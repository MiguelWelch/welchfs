<!DOCTYPE html>
<html>
<head>
    <title>AWS SDK for JavaScript - Sample Application</title>
    <script src="https://sdk.amazonaws.com/js/aws-sdk-2.1.12.min.js"></script>
</head>
	
<body>
  <div id="navigation"></div>
  <div id="listing"></div>
<script type="text/javascript" src="https://ajax.googleapis.com/ajax/libs/jquery/3.1.1/jquery.min.js"></script>
<script type="text/javascript">
  	// var S3BL_IGNORE_PATH = true;
	// var BUCKET_NAME = 'BUCKET';
  	var BUCKET_URL = 'https://s3.us-east-2.amazonaws.com/www.welchfs.me';
  	// var S3B_ROOT_DIR = 'SUBDIR_L1/SUBDIR_L2/';
  	// var S3B_SORT = 'DEFAULT';
  	// var EXCLUDE_FILE = 'clean_index.html';
  	// var AUTO_TITLE = true;
  	// var S3_REGION = for us-east-2
</script>
<script src="/list.js"></script>
<script type="text/javascript" src="https://rawgit.com/rufuspollock/s3-bucket-listing/gh-pages/list.js"></script>

	<input type="file" id="file-chooser" />
	<button id="upload-button" style="display:inline">Upload to S3</button>
	<div id="results"></div>
	<div id="fb-root"></div>
<script type="text/javascript">
	var appId = '135982260427318';
	var roleArn = 'arn:aws:iam::120280756888:role/welchfs.meRole';
	var bucketName = 'www.welchfs.me';
	AWS.config.region = 'us-east-2';
	var fbUserId;
	var bucket = new AWS.S3({
params: {
		Bucket: bucketName
		}
});
	var fileChooser = document.getElementById('file-chooser');
	var button = document.getElementById('upload-button');
	var results = document.getElementById('results');
	button.addEventListener('click', function () {
	var file = fileChooser.files[0];
	if (file) {results.innerHTML = '';

//Object key will be facebook-USERID#/FILE_NAME

	var objKey = 'facebook-' + fbUserId + '/' + file.name;
	var params = {
				Key: objKey,
				ContentType: file.type,
				Body: file,
				ACL: 'public-read'
};
bucket.putObject(params,
	function (err, data) {
		if (err) {results.innerHTML = 'ERROR: ' + err;	
		} 
			else {listObjs();
	}			
});
	}
			else {results.innerHTML = 'Nothing to upload.';
	}	
}, 
			false);
			function listObjs() {
			var prefix = 'facebook-' + fbUserId;
			bucket.listObjects({
			Prefix: prefix
},
			function (err, data) {
			if (err) {
			results.innerHTML = 'ERROR: ' + err;
	}		else {
			var objKeys = "";
			data.Contents.forEach(function (obj) 
			{objKeys += obj.Key + "<br>";
			});
results.innerHTML = objKeys;
	}
});
	}
/*!
* Login to your application using Facebook.
* Uses the Facebook SDK for JavaScript available here:
* https://developers.facebook.com/docs/javascript/gettingstarted/
*/
window.fbAsyncInit = 
					function () {
					FB.init({
					appId: appId
});
	FB.login
	(function (response) {
	bucket.config.credentials = new AWS.WebIdentityCredentials({
	ProviderId: 'graph.facebook.com',
	RoleArn: roleArn,
	WebIdentityToken: response.authResponse.accessToken
});
		fbUserId = response.authResponse.userID;
		button.style.display = 'block';
});
	};
// Load the Facebook SDK asynchronously
	(function (d, s, id) {
	var js, fjs = d.getElementsByTagName(s)[0];
	if (d.getElementById(id)) {
	return;
}
	js = d.createElement(s);
	js.id = id;
	js.src = "//connect.facebook.net/en_US/all.js";
	fjs.parentNode.insertBefore(js, fjs);
}	
	(document, 'script', 'facebook-jssdk'));
</script>
</body>
</html>
