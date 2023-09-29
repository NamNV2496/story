# send Email through gmail via SMTP
<img src="blog/java/img/sendEmail2.png" style="display: block; margin-right: auto; margin-left: auto;">

    // access
    https://myaccount.google.com/security

<img src="blog/java/img/sendEmail5.png" style="display: block; margin-right: auto; margin-left: auto;">

create new app password to create password

<img src="blog/java/img/sendEmail3.png" style="display: block; margin-right: auto; margin-left: auto;">


<img src="blog/java/img/sendEmail6.png" style="display: block; margin-right: auto; margin-left: auto;">
<img src="blog/java/img/sendEmail7.png" style="display: block; margin-right: auto; margin-left: auto;">

    // CURL
    curl --location --request GET 'http://localhost:8080/send' \
    --header 'Content-Type: application/json' \
    --data-raw '{
    "recipient": "abc@gmail.com",
    "msgBody": "hello. this is body content of email",
    "subject": "subject test"
    }'

Result

<img src="blog/java/img/sendEmail.png" style="display: block; margin-right: auto; margin-left: auto;">

# Send with attachment


    curl --location 'http://localhost:8080/sendWithAttachLink' \
    --header 'Content-Type: application/json' \
    --data-raw '{
    "recipient": "abc@gmail.com",
    "msgBody": "hello. this is body content of email",
    "subject": "subject test",
    "attachment": "link of file Ex: /C:/Users/Downloads/phan-loai-nui-1-750x500.jpg"
    }'


# Attach a file

    curl --location 'http://localhost:8080/sendWithAttach' \
    --form 'attachment=@"/C:/Users/Downloads/phan-loai-nui-1-750x500.jpg"' \
    --form 'recipient="abc@gmail.com"' \
    --form 'subject="fasdf"' \
    --form 'msgBody="hello this is example"'

<img src="blog/java/img/sendEmail4.png" style="display: block; margin-right: auto; margin-left: auto;">
