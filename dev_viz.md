#Accessing files from git:
##From javascript:
Using jquery 


```javascript
$.ajax({
  url:'git/download',
  data:{'path':repo, 'name':name}
}).done(function(data){
  //do something with data
}).fail(function(status){
  //something went wrong
});
```

```repo``` is the tokenized repository information located in the address bar,
```name``` is the file name


```data``` will be a string representation of the file contents. If expecting json it will need to be parsed with 
```javascript
var jsonData = JSON.parse(data);
```


##From java

```java
ADBioRepository repo = new AdBioRepository(new File(<path to repository>));
File f = repo.getFile("name of file");
```

Or

```java
ADBioRepository repo = new AdBioRepository();
File f = repo.getFile("name of file","path to local git repository");
```

If file not found f will be null, otherwise it will be a file descriptor for the file.




To create a file
```java
ADBioRepository repo = new AdBioRepository(new File(<path to repository>));
File f = repo.createFile(“subPath”,”name”);
repo.addFile(f.getName(),subPath,CredentialsProvider,message);
```

```createFile``` function will create the file locally with the sub path being folders in ```<path to repository>```. ```name``` is the name for the file you want to create. ```addFile``` function  adds the file to the repository. It requires a CredentialsProvider from JGit because it will push the file to the git repository.


#Creating new Html pages

The first javascript to load is adbio.credentials.js
```html
<script src="js/adbio.credentials.js"></script>
```
Then
```html
<script src="js/adbio.nav.menu.js"></script>
```
Then have this:
```html
<script type="text/javascript">
$( document ).ready(function() {
  $.ajax({
        url:'git/repo/'+repo+'?host='+host,
        dataType : 'json'}
  ).done(function(server_data){
    var project = server_data;
        if(!window.navHeader)
          window.navHeader = new NavHeader(window.credentials,project,'');
  });
});
```
```repo``` is the tokenized string in the query string, ```host``` is the host located in the query string.
```server_data``` is the project information, then creating a new NavHeader with this project will allow you to access the project information from it.

```javascript
var project = window.navHeader.project;
```
