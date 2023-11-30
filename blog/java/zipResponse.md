# Zip response

- Use ObjectMapper to convert list string to string

- Create ByteArrayOutputStream to link to zip file. This varible will help browser link to download `ByteArrayOutputStream baos = new ByteArrayOutputStream();`

- Create zip file form ByteArrayOutputStream in above `ZipOutputStream zos = new ZipOutputStream(baos)`

- Create a file `data.json` by `ZipEntry entry = new ZipEntry("data.json");`

- Add `data.json` to zip file `zos.putNextEntry(entry);`

- Write data to json `zos.write(jsonData.getBytes());`

- close `data.json` file

- Add httpHeader to return context-type `application/octet-stream`

<img src="blog/java/img/zipResponse1.png" style="display: block; margin-right: auto; margin-left: auto;">


- link to json file in resources `ClassPathResource jsonResource = new ClassPathResource("large-file.json");`

- create InputStream from json file `InputStream jsonStream = jsonResource.getInputStream();`

- read all data in json file.

- Create ByteArrayOutputStream to link to zip file. This varible will help browser link to download `ByteArrayOutputStream baos = new ByteArrayOutputStream();`

- Create zip file form ByteArrayOutputStream in above `ZipOutputStream zos = new ZipOutputStream(baos)`

- Create a file `data.json` by `ZipEntry entry = new ZipEntry("data.json");`

- Add `data.json` to zip file `zos.putNextEntry(entry);`

- Write data to json `zos.write(jsonData.getBytes());`

- close `data.json` file

- Add httpHeader to return context-type `application/octet-stream`

<img src="blog/java/img/zipResponse2.png" style="display: block; margin-right: auto; margin-left: auto;">



## Access in browser

API get file with data is a list

// curl --location 'http://localhost:8080/api/download-json-list-zip'


API get file with data in JSON from 24.9MB -> 3.66MB

// curl --location 'http://localhost:8080/api/zipJson'


