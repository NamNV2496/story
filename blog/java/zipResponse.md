# Zip response

- Use ObjectMapper to convert list string to string

- Create ByteArrayOutputStream to link to zip file. This varible will help browser link to download `ByteArrayOutputStream baos = new ByteArrayOutputStream();`

- Create zip file form ByteArrayOutputStream in above `ZipOutputStream zos = new ZipOutputStream(baos)`

- Create a file `data.json` by `ZipEntry entry = new ZipEntry("data.json");`

- Add `data.json` to zip file `zos.putNextEntry(entry);`

- Write data to json `zos.write(jsonData.getBytes());`

- close `data.json` file

- Add httpHeader to return context-type `application/octet-stream`


```java
@GetMapping("/download-json-list-zip")
    public ResponseEntity<byte[]> downloadJsonListZip() throws IOException {
        // Create a list of objects (for example, strings)
        List<String> dataList = Arrays.asList("Item 1", "Item 2", "Item 3");

        // Convert the list to JSON
        ObjectMapper objectMapper = new ObjectMapper();
        String jsonData = objectMapper.writeValueAsString(dataList);

        // Create a ByteArrayOutputStream to hold the zip content
        ByteArrayOutputStream baos = new ByteArrayOutputStream();
        try (ZipOutputStream zos = new ZipOutputStream(baos)) {
            // Add a JSON file to the zip
            ZipEntry entry = new ZipEntry("data.json");
            zos.putNextEntry(entry);
            zos.write(jsonData.getBytes());
            zos.closeEntry();
        }

        // Set up HTTP headers then browser will download automatically
        HttpHeaders headers = new HttpHeaders();
        headers.setContentType(MediaType.APPLICATION_OCTET_STREAM);
        headers.setContentDispositionFormData("attachment", "data.zip");

        // Return the zip file as a byte array in the response body
        return new ResponseEntity<>(baos.toByteArray(), headers, HttpStatus.OK);
    }
```

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


```java
    @GetMapping("/zipJson")
    public ResponseEntity<byte[]> zipJsonFile() throws IOException {

        // Read JSON file from resources
        ClassPathResource jsonResource = new ClassPathResource("large-file.json");

//        String zipFilePath = "path/to/your/destination/file.zip";
//        FileOutputStream fos = new FileOutputStream(zipFilePath);
//        ZipOutputStream zipOut = new ZipOutputStream(fos);

        InputStream jsonStream = jsonResource.getInputStream();
        byte[] jsonData = jsonStream.readAllBytes();

        // Create a ByteArrayOutputStream to hold the zipped content
        ByteArrayOutputStream baos = new ByteArrayOutputStream();
        try (ZipOutputStream zipOut = new ZipOutputStream(baos)) {
            // Add JSON file to the zip
            ZipEntry entry = new ZipEntry("data.json");
            zipOut.putNextEntry(entry);
            zipOut.write(jsonData);
            zipOut.closeEntry();
        }

        // Set up response headers
        HttpHeaders headers = new HttpHeaders();
        headers.setContentType(MediaType.APPLICATION_OCTET_STREAM);
        headers.setContentDispositionFormData("attachment", "data.zip");

        // Return the zipped content as a byte array
        return ResponseEntity.ok()
            .headers(headers)
            .body(baos.toByteArray());
    }
```

<img src="blog/java/img/zipResponse2.png" style="display: block; margin-right: auto; margin-left: auto;">


## Access in browser

API get file with data is a list

// curl --location 'http://localhost:8080/api/download-json-list-zip'


API get file with data in JSON from 24.9MB -> 3.66MB

// curl --location 'http://localhost:8080/api/zipJson'


