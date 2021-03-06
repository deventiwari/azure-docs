---
title: Previewing Drive Usage for an Export Job | Microsoft Docs
description: Learn how to preview the list of blobs you have selected for an export job in the Azure Import-Export Service
author: renashahmsft
manager: aungoo
editor: tysonn
services: storage
documentationcenter: ''

ms.assetid: 7707d744-7ec7-4de8-ac9b-93a18608dc9a
ms.service: storage
ms.workload: storage 
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2015
ms.author: renash

---

# Previewing Drive Usage for an Export Job
Before you create an export job, you need to choose a set of blobs that are to be exported. The Microsoft Azure Import/Export service allows you to use a list of blob paths or blob prefixes to represent the blobs you have selected.  
  
 Next you need to determine how many drives you need to send. The Microsoft Azure Import/Export tool provides the `PreviewExport` command to preview drive usage for the blobs you selected, based on the size of the drives you are going to use. You can specify the following parameters:  
  
|Command-line Option|Description|  
|--------------------------|-----------------|  
|**/logdir:**<LogDirectory\>|Optional. The log directory. Verbose log files will be written to this directory. If no log directory is specified, the current directory will be used as the log directory.|  
|**/sn:**<StorageAccountName\>|Required. The name of the storage account for the export job.|  
|**/sk:**<StorageAccountKey\>|Required if and only if a container SAS is not specified. The account key for the storage account for the export job.|  
|**/csas:**<ContainerSas\>|Required if and only if a storage account key is not specified. The container SAS for listing the blobs to be exported in the export job.|  
|**/ExportBlobListFile:**<ExportBlobListFile\>|Required. Path to the XML file containing list of blob paths or blob path prefixes for the blobs to be exported. The file format used in the `BlobListBlobPath` element in the [Put Job](/rest/api/storageservices/importexport/Put-Job) operation of the Import/Export Service REST API.|  
|**/DriveSize:**<DriveSize\>|Required. The size of drives to use for an export job, *e.g.*, 500GB, 1.5TB.|  
  
The following example demonstrates the `PreviewExport` command:  
  
```  
WAImportExport.exe PreviewExport /sn:bobmediaaccount /sk:VkGbrUqBWLYJ6zg1m29VOTrxpBgdNOlp+kp0C9MEdx3GELxmBw4hK94f7KysbbeKLDksg7VoN1W/a5UuM2zNgQ== /ExportBlobListFile:C:\WAImportExport\mybloblist.xml /DriveSize:500GB    
```  
  
The export blob list file may contain blob names and blob prefixes, as shown here:  
  
```xml 
<?xml version="1.0" encoding="utf-8"?>  
<BlobList>  
<BlobPath>pictures/animals/koala.jpg</BlobPath>  
<BlobPathPrefix>/vhds/</BlobPathPrefix>  
<BlobPathPrefix>/movies/</BlobPathPrefix>  
</BlobList>  
```

The Azure Import/Export tool lists all blobs to be exported and calculates how to pack them into drives of the specified size, taking into account any necessary overhead, then estimates the number of drives needed to hold the blobs and drive usage information.  
  
Here is an example of the output, with informational logs omitted:  
  
```  
Number of unique blob paths/prefixes:   3  
Number of duplicate blob paths/prefixes:        0  
Number of nonexistent blob paths/prefixes:      1  
  
Drive size:     500.00 GB  
Number of blobs that can be exported:   6  
Number of blobs that cannot be exported:        2  
Number of drives needed:        3  
        Drive #1:       blobs = 1, occupied space = 454.74 GB  
        Drive #2:       blobs = 3, occupied space = 441.37 GB  
        Drive #3:       blobs = 2, occupied space = 131.28 GB    
```  
  
## See Also  
[Azure Import-Export Tool Reference](storage-import-export-tool-how-to-v1.md)