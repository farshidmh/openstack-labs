### Generating a Temporary URL for an Object

Temporary URLs allow you to create time-limited URLs to access objects without authentication.

### Time Estimate:

You should be able to complete this lab in less than `15 minutes`.


**Setting a Temporary URL Key**  
Set a key for your account to generate and validate temporary URLs:

```bash
swift post -m "Temp-URL-Key: mykey"
```

Replace `mykey` with a secure key of your choice.

**Generating the Temporary URL**  
Generate a URL for `myfile.txt` in `mycontainer`, valid for 3600 seconds (1 hour):

```bash
swift tempurl GET 3600 /v1/AUTH_myaccount/mycontainer/myFile.txt mykey
```

Replace `AUTH_myaccount` with your OpenStack Swift account identifier.

To get the `AUTH_myaccount` use the following command:

```bash
openstack project list
```

The `ID` column contains the `AUTH_myaccount` value.

**Accessing the Object via Temporary URL**  
Access `myfile.txt` using the generated URL for the specified duration.

Url would look like this:

```bash
http://<swift-proxy-node-ip>:8080/v1/AUTH_myaccount/mycontainer/myFile.txt?temp_url_sig=0f2b3c4d5e6f7g8h9i0j&temp_url_expires=1234567890
```

---

### Conclusion:

In this lab, you've learned the fundamental operations of OpenStack Swift using the CLI. This includes creating containers, manipulating objects, and securely sharing objects using temporary URLs. This provides a foundational understanding of Swift's Object Storage service.

---

Whenever you're ready for the next lab, please let me know!