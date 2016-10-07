<a name="Chapter2"></a>
# Chapter 2 – Export a model to another format

Today interchangeable formats are (not exclusive) OBJ, FBX, DAE, STL, ...
FBX is used by Unity as a primary format, and the FBX SDK can read/write OBJ and DAE.
The Model Derivative API supports OBJ today, and will support FBX in a few weeks time.

For our pipeline, we will focus on OBJ, with FBX convertion as an additional step.


<a name="RequestOBJ"></a>
## Request the Model Derivative to produce OBJ

After the translate command line #474, add the following code (this new command is a copy'n past of the translate 
command with few changes explained below):

```
program
	.command ('obj')
	.description ('translate the file to obj')
	.arguments ('<fileKey>')
	.option ('-f, --file', 'fileKey represent the final objectKey on OSS vs a local fileKey')
	.option ('-e, --entry <rootFile>', 'rootFile: which file to start from')
	.option ('-m, --model <rootKey>', 'rootKey: which model to start from')
	.action (function (fileKey, options) {
		var bucketKey =readBucketKey () ;
		if ( !checkBucketKey (bucketKey) )
			return ;
		if ( options.file === undefined )
			fileKey =readFileKey (bucketKey, fileKey) ;
		if ( options.model === undefined )
			return (console.log ('Missing model parameter')) ;
		console.log ('Request file to be translated to OBJ') ;
		var urn =URN (bucketKey, fileKey) ;
		options.entry =options.entry || fileKey ;
		access_token (function (/*access_token*/) {
			var job ={
				"input": {
					"urn": urn,
					"compressedUrn": (path.extname (fileKey).toLowerCase () === '.zip'),
					"rootFilename": options.entry
				},
				"output": {
					"formats": [
						{
							"type": "obj",
							"advanced": {
								"modelGuid": options.model, // from metadata
								"objectIds": [ -1 ]
							}
						}
					]
				}
			} ;
			var bForce =true ;
			md.translate (job, { 'xAdsForce': bForce }, function (error, data, response) {
				errorHandler (error, data, 'Failed to register file for translation') ;
				httpErrorHandler (response, 'Failed to register file for translation') ;
				console.log ('Registration successfully submitted.') ;
				console.log (JSON.stringify (data, null, 4)) ;
			}) ;
		}) ;
	}) ;
```

The important piece in this code, is actually where you tell the WEB service you want an OBJ extraction
of the model. By default the Autodesk Model Derivative service will extract to SVF.

```
{
	"type": "obj",
		"advanced": {
		"modelGuid": options.model, // from metadata
		"objectIds": [ -1 ]
	}
}
```

The objectIds is -1 which means we want to extract everything from the model. But you can eventually extract selected 
objects, by specifying the ID of the objects to extract. For example extract a chair from a house project.


<a name="DownloadOBJ"></a>
## Download the OBJ file

Add the following code to the project after the command you created in the previous step.
```
program
	.command ('dl')
	.description ('download the obj')
	.arguments ('<fileKey>')
	.option ('-f, --file', 'fileKey represent the final objectKey on OSS vs a local fileKey')
	.option ('-u, --urn <derivedURN>', 'derived urn to download')
	.action (function (fileKey, options) {
		var bucketKey =readBucketKey () ;
		if ( !checkBucketKey (bucketKey) )
			return ;
		if ( options.file === undefined )
			fileKey =readFileKey (bucketKey, fileKey) ;
		if ( options.urn === undefined )
			return (console.log ('Missing urn parameter')) ;
		console.log ('Request OBJ file download') ;
		var urn =URN (bucketKey, fileKey) ;
		options.entry =options.entry || fileKey ;
		access_token (function (/*access_token*/) {
			var wstream =fs.createWriteStream (options.entry + '.obj') ;
			md.getDerivativeManifest (urn, options.urn.replace (/=/gi, ''), {}, function (error, data, response) {
				errorHandler (error, data, 'Failed to d/l file') ;
				httpErrorHandler (response, 'Failed to d/l file') ;
				//console.log (data) ;
				console.log ('obj downloaded successfully') ;
			})
			.pipe (wstream) ;
		}) ;
	}) ;
```

This code only download content as a stream which we are saving into a file. Here it is an OBJ file, bu it could be 
anything like a thumnbail, database, ...


<a name="TimeToRun"></a>
## Time to test our pipeline

Put your client ID and secret in the placeholders below.

```
FORGE_CLIENT_ID=<client_id> FORGE_CLIENT_SECRET=<client_secret> node forge-cb.js 2legged
node forge-cb.js bucketCreate <unique-name>  ' something unique - to do only once
node forge-cb.js upload samples/wrench.f3d
node translate wrench.f3d
node translateProgress wrench.f3d  ' until is says success (complete)
node metadata wrench.f3d  ' it will display a list of scene guid you can extract, choose 1 to use in the next call
node obj wrench.f3d -g <guid>
node translateProgress wrench.f3d  ' until is says success (complete)
node manifest wrench.f3d  ' scroll to the obj section and find the obj URN (starts with urn:adsk.viewing:fs.file:)
node dl wrench.f3d -u <urn>
ls -l wrench.f3d.obj
```


=========================
[Home](../README.md)