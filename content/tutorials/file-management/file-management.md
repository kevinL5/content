---
alias: eefoovo2ah
path: /docs/tutorials/file-management
layout: TUTORIAL
preview: file-management-preview.png
title: Managing files in a Graphcool project
description: Upload, download, rename or delete files that are hosted at your GraphQL backend.
tags:
  - file-management
  - client-apis
  - queries
  - mutations
  - plain-http
related:
  further:
    - eer4wiang0
    - koo4eevun4
  more:
    - dah6aifoce
    - goij0cooqu
---

# Managing files in a Graphcool project

In this guide we will use the file management system at Graphcool to upload, download, rename and delete files.

> To get the most out of this guide, you already should be familiar with the Graphcool platform. The [introductory guide](!alias-thaeghi8ro) on how to set up a GraphQL backend in less than 5 minutes is a great way to do that!

## 1. The File Management Workflow

Every project at Graphcool comes with a `File` model that is used for file management. Whenever you upload a new file, a `File` node is created. Afterwards, your file will be available at a secret location that is stored in that node, amongst other meta information. While access to the file node is protected with the same role-based permission system as all other nodes, everyone with knowledge of the secret or url of a file can access it!

## 2. Uploading a File

There are two ways to upload files

* uploading a file with plain HTTP
* uploading a file with the Client APIs

You can upload files up to 50MB each.

### 2.1 Uploading a File with plain HTTP

#### 2.1.1 curl

Let's upload our first file now! Pick a funny picture or similar and navigate to the folder containing it, for example

```sh
cd ~/Downloads
```

Using `curl` you can then execute

```sh
curl -X POST 'https://api.graph.cool/file/v1/__PROJECT_ID__' -F "data=@example.png"
```

after replacing `__PROJECT_ID__` with your project id and `example.png` with the name of your file. You can copy your file endpoint from inside your project.

The response could look something like this:

```JSON
{
  "secret": "__SECRET__",
  "name": "example.png",
  "size": <omitted>,
  "url": "https://files.graph.cool/__PROJECT_ID__/__SECRET__",
  "id": <omitted>,
  "contentType": "image/png"
}
```

Go to the url in the response to verify that the image file was correctly uploaded. To read more on the fields of the response read the reference documentation on the [`File` model](!alias-uhieg2shio#file-model).


#### 2.1.2 AJAX

With AJAX, you could have this file upload form:

```js
<form>
    <input type="file" id="file" name="file">
    <input type="submit">
</form>
```

Then you can upload the file like this:

```js
$( 'form' ).submit(function ( e ) {
  // prepare the file data
  var data = new FormData()
  data.append( 'file', $( '#file' )[0].files[0] )

  // do a post request
  var xhr = new XMLHttpRequest()
  xhr.open('POST', 'https://api.graph.cool/file/v1/__PROJECT_ID__', true)
  xhr.onreadystatechange = function ( response ) {}
  xhr.send( data )
  e.preventDefault()
})
```

Remember to replace the file endpoint. You can copy your file endpoint from inside your project.

> Regardless of your http library, make sure to **set the header `Content-Type` to `multipart/form-data`** and **use the file key `data`**.

### 2.2 Uploading a File with the Client APIs

Coming soon.

## 3. Modifying an existing File

To modify an existing file, you can use the `updateFile` mutation available in both the [Relay API](!alias-aizoong9ah) and [Simple API](!alias-heshoov3ai). Let's rename the file we just uploaded. Grab the id of the according file node and execute the `updateFile` mutation. With the Simple API, this would look like this:

```graphql
updateFile(id: "<your-file-id>" name: "newName.png") {
  name
}
```

If you want to delete a file, use the `deleteFile` mutation and provide the id of the file you want to delete.

## 4. Downloading an existing File

To download a file, all you need to know is its url. If you have the id of the file, you can run the `File` query to fetch it. In the Simple API, it looks like this:

```graphql
File(id: "<your-file-id>") {
  url
}
```

Then you could use curl to download it using

```sh
curl -O -J <your-file-url>
```

## 5. Next steps

Congratulations, you now should be able to work with files in your GraphQL backend on your own. If you want to read more about the file management system in detail, find out more in the [documentation](!alias-eer4wiang0).

Remember that the `File` model is just another model in your backend. That means that you can create new fields and relations just as with any other model. For example, creating a relation `FileOwner` between the `User` and the `File` model could come in handy. If you want to read more on relations in general the [relations guide](!alias-daisheeb9x) might be useful to you.
