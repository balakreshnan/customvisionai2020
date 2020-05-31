# Image Data Labelling - large volume using Machine Learning

## Use Case

To build object detection model using custom vision or ML assisted data labelling we need to resize images which are large. 
Here is a script to resize images to 6MB. The data set i have varies from 4MB to 14MB. When i tried ML assisted labelling it got errored out by going over the memory.

## Challenge

Image resize can be accomplished even in a local computer, but here is the challenge takes a longer time and occupies's CPU of the laptop or computer locally. 

## Solution

To make the solution not dependant on local resource and to increase scale as needed i choosed to run this code in Azure machine learning services workspace with compute instance and Notebook. This combination provides me a isolated environment in cloud and images can be stored in the cloud as well in a blob storage. This avoid network latency and all the processing in done in cloud. Now the size of compute instance is up to how much we want to use and can be scaled as needed.

Moving the files to cloud blob storage can be achieved by any data movement tools like Azure data factory to move the data.

## Architecture

![alt text](https://github.com/balakreshnan/customvisionai2020/blob/master/images/mlassitresize.jpg "Architecture")

## Steps to resize

- Create a Blob Storage account
- Create a container
- upload images or i moved from another using Azure Data Factory Copy wizard
- Create a Azure ML services workspace
- Create a compute instance
- Create a new notebook
- Select Python version 3

## Code segment to resize

First and foremost lets update the new sdk for azure blob storage 

```
pip install azure-storage-blob --upgrade
```

Now import necessary imports

```
import os, uuid
from azure.storage.blob import BlobServiceClient, BlobClient, ContainerClient
```

Now connect and list all containers. 
Substitute your Storage account name and key in the connection string.

```
import os, uuid
from azure.storage.blob import BlobServiceClient, BlobClient, ContainerClient

try:
    print("Azure Blob storage v12 - Python quickstart sample")
    # Quick start code goes here
    AZURE_STORAGE_CONNECTION_STRING="DefaultEndpointsProtocol=https;AccountName=xxxx;AccountKey=xxxxxxxxxxxxxxxxxxxx;EndpointSuffix=core.windows.net"
    
    # Create the BlobServiceClient object which will be used to create a container client
    blob_service_client = BlobServiceClient.from_connection_string(AZURE_STORAGE_CONNECTION_STRING)
    
    # List containers
    print("\nListing containers...")
    containers_list = blob_service_client.list_containers()
    for c in containers_list:
        print("\t" + c.name)

except Exception as ex:
    print('Exception:')
    print(ex)
```

Now you should see a list of files

List all the source images files to make sure there are images in the blob

```
print("\nListing blobs...")

# List the blobs in the container
blob_list = blob_service_client.get_container_client("Containername").list_blobs()
for blob in blob_list:
    print("\t" + blob.name)
```

Import for image manupulation and also source and destination container settings
To resize to different image size please change the variable newSize

```
from PIL import Image
from pathlib import Path

DEST_FILEIMG = "img1.jpg"
RE_FILEIMG = "imgresize1.jpg"
newSize = 6000000 # bytes equal to 6MB

filename = "image1.jpj"

container_client = blob_service_client.get_container_client("sourcecontainer")
container_client1 = blob_service_client.get_container_client("destinationcontainer")
```

Here is the code to read one at a time and resize it

```
blob_list = blob_service_client.get_container_client("excelon1").list_blobs()
for blob in blob_list:
        #print("\t" + blob.name)
        if("jpg" in blob.name):
            print("\t" + blob.name)
            blob_client = container_client.get_blob_client(blob.name)
            download_stream = blob_client.download_blob()
            #print("File name" + blob.name + str(download_stream.readall()) + "\t")
            with open(DEST_FILEIMG, "wb") as my_blob_img:
                my_blob_img.write(download_stream.readall())
                
            img = Image.open(DEST_FILEIMG)
            width, height = img.size
            fileSize = os.stat(DEST_FILEIMG).st_size
            resizeFactor = 1 - (fileSize - newSize)/fileSize
            #resizeFactor = 1 - (fileSize - newSize)/fileSize
            if(fileSize > newSize):
                resizeFactor = 1 - (fileSize - newSize)/fileSize
                if(resizeFactor > 1): resizeFactor = 1
                newX = round(img.size[0] * resizeFactor)
                newY = round(img.size[1] * resizeFactor)

                img = img.resize((newX,newY), Image.ANTIALIAS)
                img.save(RE_FILEIMG)

                local_file_name = blob.name
                blob_client1 = blob_service_client.get_blob_client(container="destinatoncontainername", blob=local_file_name)
                with open(RE_FILEIMG, "rb") as data:
                    blob_client1.upload_blob(data)
```

I preserved the same name so that ic an compare if needed. Also i am doing single file at a time, parallelizing this might be a another great article for future.

Here is to count the size

```
print("\nListing blobs...")
count = 0

# List the blobs in the container
blob_list = blob_service_client.get_container_client("destinationcontainername").list_blobs()
for blob in blob_list:
    #print("\t" + blob.name)
    count += 1
    
print("Total Files in container: " + str(count))
```
## Time Taken

For my above run it took about 30 minutes to process about 1000 images varying in size.

Thanks