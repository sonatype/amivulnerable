# Am I Vulnerable?

Upload an application, get an Application Health Check report emailed to you

## Updating S3 policy

This tool uses an HTML form to POST files to S3 for processing. For this to
work, you have to define a policy and annotate the form appropriately. Perform
the following steps when changing policy.

### Update policy.json when you change the upload form

Every form field, with the exception of AWSAccessKeyId, signature, policy, and
file, needs a cooresponding field in `policy.json`. Edit that file first.

### Generate a Base64 representation of policy.json

    $ base64 policy.json
    ewogICJleHBpcmF0aW9uIjogIjIwMTgtMDEtMDFUMDA6MDA6MDBaIiwKICAiY29uZGl0aW9ucyI6IFsKICAgIHsiYnVja2V0IjogInVwbG9hZHNmb3JhbWl2dWxuZXJhYmxlIn0sCiAgICBbInN0YXJ0cy13aXRoIiwgIiRrZXkiLCAidXBsb2Fkcy8iXSwKICAgIHsiYWNsIjogInByaXZhdGUifSwKICAgIHsic3VjY2Vzc19hY3Rpb25fcmVkaXJlY3QiOiAic3VjY2Vzcy5odG1sIn0sCiAgICBbInN0YXJ0cy13aXRoIiwgIiR4LWFtei1tZXRhLWVtYWlsIiwgIiJdLAogICAgWyJzdGFydHMtd2l0aCIsICIkeC1hbXotbWV0YS1wYXNzd29yZCIsICIiXQogIF0KfQo=

### Sign the policy with your AWS secret access key

    $ irb
    > require 'base64'
    > require 'openssl'
    > require 'digest/sha1'
    > Base64.encode64(OpenSSL::HMAC.digest(OpenSSL::Digest::Digest.new('sha1'), "your-aws-secret-access-key", "base64-representation-of-your-policy")).gsub("\n","")
    => "Ym4HY6EFuMTbN0I7wrr9/t8rJDM="

### Update the policy and signature fields in the upload form

Finally, take the Base64 representation of your policy, and the signature you
generated, and replace the corresponding fields in the upload form.
