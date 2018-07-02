# Student-Course-Registration-Website-using-DynamoDB-hosted-in-AWS

I developed and deployed a simple student course registration website using Lambda, DynamoDB and S3 comcepts in AWS.

S3 – to upload the information of the student and to host website.
 
Lambda Function – trigger whenever an object is created in input S3 bucket to put the data in dynamo table.

Dynamo Db – Stores the student-id and file-path to restore the file from S3 when student want to view their registered courses.

#-----------------Code--------------------#


<!DOCTYPE html>
<html>
<head>
<title>Student Registration</title>
<script src="https://sdk.amazonaws.com/js/aws-sdk-2.3.0.min.js"></script>
<!-- Latest compiled and minified CSS -->
<link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css" integrity="sha384-BVYiiSIFeK1dGmJRAkycuHAHRg32OmUcww7on3RYdg4Va+PmSTsz/K68vbdEjh4u" crossorigin="anonymous">
</head>
<body>
<div class="container well">
<div class="row" align="center"> 
<h1 style="align: center;">Student Registration</h1>
</div>
<br/>
<div class="row" align="center"> 
<table>
<tr>
<td><b>Student ID: </b></td>
<td><input type="text" id="sid"/></td>
</tr>
<tr colspan="2">
<td>
<fieldset>
<legend>Courses:</legend>
<input id="ccCb" type="checkbox" name="course" value="CC" />Cloud Computing<br/>
<input id="ppCb" type="checkbox" name="course" value="PP" />Python Programming<br/>
<input id="osCb" type="checkbox" name="course" value="OS" />Operating Systems<br/>
<input id="aoaCb" type="checkbox" name="course" value="AOA" />Analysis of Algorithms<br/>
<input id="wadCb" type="checkbox" name="course" value="WAD" />Web Application Development<br/>
<input id="caCb" type="checkbox" name="course" value="CA" />Computer Architecture<br/>
<input id="cvCb" type="checkbox" name="course" value="CV" />Computer Vision
</fieldset>
</td>
</tr>
</table>
</div> 
<br />
<div class="row" align="center">
<button class="btn btn-primary" id="registerBtn">Register</button>
<button class="btn btn-primary" id="viewBtn">View</button>
<br/>	
<br/>
<ul id="html-list"style="list-style: none;"></ul>
</div>
</div>
<script>
 AWS.config.update({
	accessKeyId: '<your accessKeyID>',
	secretAccessKey: '<your secretAccessKey>',
	region: '<your region>' // eg: 'us-east-1'
 });
 var s3 = new AWS.S3({params: {Bucket: '<your bucket name>'}});
 var Dynamo = new AWS.DynamoDB.DocumentClient({region: 'us-east-1'});
    // Fetch the Student ID from the input
    function getStudentId() {
      return document.getElementById('sid').value;
    }

    // Grab a reference to the upload button
    let registerButton = document.getElementById('registerBtn');	  
	
    // Make the button respond to clicks
    registerButton.addEventListener('click', function() {	
	let studentId = getStudentId();
	let selectedCourse = "";
	let count = 0;
	if (!studentId) {
        alert("You need to enter a student id");
        return;
      }
	if(document.getElementById("ccCb").checked) {
	 selectedCourse += "Cloud Computing<br/>";
	 count += 1;
	}
	if(document.getElementById("ppCb").checked) {
	 selectedCourse += "Python Programming<br/>";
	 count += 1;
	}
	if(document.getElementById("osCb").checked) {
	 selectedCourse += "Operating Systems<br/>";
	 count += 1;
	}
	if(document.getElementById("aoaCb").checked) {
	 selectedCourse += "Analysis of Algorithms<br/>";
	 count += 1;
	}
	if(document.getElementById("wadCb").checked) {
	 selectedCourse += "Web Application Development<br/>";
	 count += 1;
	}
	if(document.getElementById("caCb").checked) {
	 selectedCourse += "Computer Architecture<br/>";
	 count += 1;
	}
	if(document.getElementById("cvCb").checked) {
	 selectedCourse += "Computer Vision<br/>";
	 count += 1;
	}
	if(count == 0) {
	 alert("Please select atleast one course for registration");
	 return;
	}
	if(count > 5) {
	 alert("You cannot choose more than 5 course");
	 return;
	}
      let params = {
        Key: studentId + '/' + studentId,
        ContentType: "text/html",
        Body:  selectedCourse,
        ACL: 'public-read'
      };
	   s3.upload(params, function(err, data) {
        if (err) {
		  //alert("Error occured");
          alert(err);
        } else {
          alert("Registered successfully!");
        }
      });
	  
     });
	 
	 let viewButton = document.getElementById('viewBtn');
	 viewButton.addEventListener('click', function() {
      let studentId = getStudentId();
      if (!studentId) {
        alert("You need to enter a student id");
        return;
      }
	  viewButton.disabled = true
	  document.getElementById('html-list').innerHTML = "";
	    let ul = document.getElementById('html-list');
      let li = document.createElement('li');
      let iframeHtml = document.createElement('iframe');
	  
      iframeHtml.src = 'https://s3.amazonaws.com/<your bucket name>/' + studentId + '/' +studentId;
      //img.style.maxWidth = "200px";

      li.appendChild(iframeHtml);
      ul.appendChild(li);    
	  
	
     });

    
</script>
</body>
</html >
