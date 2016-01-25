Cloud9 web IDE
eg. # docker run -d -p 8080:8080 -v /local-file-location/:/workspace/ -e TZ='Australia/Brisbane' -e C9USER=cloud9 C9PASS=cloud9 quimnut/cloud9ide:latest

Upon boot the name for the non-root user will be updated to the value of C9USER.