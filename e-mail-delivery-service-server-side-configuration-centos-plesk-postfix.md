---

copyright:
  years: 2014, 2020
lastupdated: "2021-02-18"

keywords: email delivery server configuration, centos, plesk, postfix

subcollection: email-delivery

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:table: .aria-labeledby="caption"}

# Configuring server-side email delivery service for CentOS, Plesk, and Postfix
{: email-delivery-server-config}

Follow these steps to configure your server to use {{site.data.keyword.SendGrid}}, the {{site.data.keyword.cloud}} email delivery service as a smart host. The following example is a standard {{site.data.keyword.cloud}} OS Reload of CentOS 6.5 with Plesk 12 and Postfix.

## Updating the Postfix configuration file
{: #email-delivery-config}

1. Locate your Postfix configuration file. A common location is /etc/postfix/main.cf.
2. Open main.cf file with a text editor 
3. Add the following to the configuration.<<Is there a particular location? The end? Can this be cut and pasted as a whole? >>

```
smtp_sasl_auth_enable = yes
smtp_sasl_password_maps = static:_Your {{site.data.keyword.SendGrid}} Username_:_Your {{site.data.keyword.SendGrid}} Password_
smtp_sasl_security_options = noanonymous
smtp_tls_security_level = encrypt
header_size_limit = 4096000
relayhost = [smtp.sendgrid.net]:587
```

3. Save and close the /etc/postfix/main.cf file.
4. Use the following command to restart Postfix
```
/etc/init.d/postfix restart
```

## Troubleshooting
{: #email-delivery-troubleshoot}

### No mechanism available

If you see the "no mechanism available" error, you may need to install libraries necessary for authentication or encryption. 

To install the libraries: 
1. Run the following command: 

  a. For Debian and Ubuntu: `apt-get install libsasl2-modules`
  b. For Red Hat and CentOS: `yum install cyrus-sasl-plain`

2. Run the following command to restart Postfix: `/etc/init.d/postfix restart`

### Port 587

<<how do you know port 587 doesn’t work? >>
If port 587 does not work, use port 2525 in the Postfix configuration. 

### Uncomment line 
<<what error should you see before doing this?>>

1. Reopen the configuration file `/etc/postfix/main.cf` and uncomment the following line: 
   `#tlsmgr unix - - n 1000? 1 tlsmgr`
