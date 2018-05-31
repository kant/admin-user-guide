# Email

## **Primary and Technical Contacts**

All PKP applications require that a primary and technical contact are configured under Setup for proper  daily operations. This has to be done for every journal, press or conference on the system. In OJS 2.x, this can be done under _Setup Step 1_. In OCS 2.x, this can be done under _Website Management Step 1_. IN OJS/OMP 3.x, this can be done under _Settings &gt; Journal &gt; Contact_.

## **Sending Mail**

By default, mail is sent through PHP's built-in `mail()` facility.

On Windows, PHP needs to be configured to send email through a SMTP server \(running either on the same machine or on another machine\).

On other platforms such as Linux and Mac OS X, PHP will sent mail using the local sendmail client, so a local MTA such as Sendmail or Postfix must be running and configured to allow outgoing mail.

See [http://www.php.net/mail](http://www.php.net/mail) for more details on configuring PHP's mail functionality.

Our software can also be configured to use an SMTP server as specified in `config.inc.php`, either with or without authentication.

## **Setting a Bounce Address**

To control the address to which a bounced emails will be sent, you need to set the envelope sender address. Enable the `allow_envelope_sender` option in the `[email]` section of the configuration file; when this option is enabled, a "Bounce Address" field appears in the Email section under Setup.

Note that this option may require changes to the server's mail server configuration so that the user the web server runs as \(e.g. "www-data"\) is trusted by the sendmail program, or an "X-Warning" header will be added to outgoing messages. Consult your mail server's documentation if outgoing mails include such a header.

For example, Sendmail keeps a list of trusted users in the file "/etc/mail/trusted-users"; other mail systems use similar files.

The command-line option used to set the envelope sender is `-f`.

## **Configuring the system to use GMail SMTP**

To use gmail SMTP to send email from OJS system. You can use the following setting in config.inc.php

For OJS 2.x:

```text
; Use SMTP for sending mail instead of mail()
smtp = On

; SMTP server settings
smtp_server = "ssl://smtp.gmail.com"
smtp_port = 465

; Enable SMTP authentication
smtp_auth = PLAIN
smtp_username = "user@gmail.com"
smtp_password = "password"

```

For OJS 3.x:

```text
; Use SMTP for sending mail instead of mail()
smtp = On

; SMTP server settings
smtp_server = smtp.gmail.com
smtp_port = 465

; Enable SMTP authentication
smtp_auth = ssl
smtp_username = "user@gmail.com"
smtp_password = "password"

```

Note that you may have to additionally configure application-specific passwords in gmail; see [https://support.google.com/accounts/answer/185833?hl=en](https://support.google.com/accounts/answer/185833?hl=en) for details.

## **Troubleshooting email problems**

If emails aren't being received by some users, the first thing to do is check to see if you yourself can receive email. Try sending an email to yourself using the system. If you received it, OJS is probably sending email fine. You should then ask the user with the problem to check their email's spam/junk folders. 

If the user can find no record whatsoever of the email being filtered as spam or junk, you may be encountering a **Sender Policy Framework** \(SPF\) validation problem with their server. You can confirm this by viewing your server's mail log to see if there are any reported receipt blockages/returns with SPF validation errors as the result.

### **Explanation of the problem, and the solution**

As of version 2.4.6, OJS included a change to the way emails are sent out. Previously, all emails were sent using the OJS user's email address in the "FROM" field. This unfortunately led to some issues with the journal's outgoing emails being flagged as "spoofed" by some email servers, since the email addresses in question \(eg. james@myinstitution.org"\) didn't match the domain name of the server sending the email \(eg. "myjournal.com"\). \(Technically, the emails were failing Sender Policy Framework \(SPF\) validation.\) Being flagged in this way is more serious than being considered spam: in many cases, the receiving email server won't even assign the email to a spam/junk queue, instead simply choosing to discard it. 

#### **Solution 1 \(OMP, OJS\):** 

To prevent this from happening, the PKP development team has adopted an email notification method similar to other web applications such as Wordpress: send all email from the system using one central email address in the "FROM" field, with the intended recipients' email addresses in the "REPLY-TO" Field. The central email address to be used by default would be the one provided in **Journal Setup Step 1.2: Principal Contact**, which should match the domain name from which the journal sends mail. \(If this email address cannot match the sending domain on a per-journal basis, an alternate email address can be configured at the site level via the OJS config.inc.php file\). In addition, a new "Email Header" setting has been provided in **Journal Setup Step 1.4: Email Identification**, which can be used to provide explanatory text to the recipient. 

To properly configure this, we suggest the following: 

* If you aren't on OJS 2.4.6+ already, upgrade. 
* Configure the email address OJS will use to send out all email using the "Principal Contact" setting in Journal Setup Step 1.2 
  * If at all possible, have your Principal Contact email address serve as a general point of contact for the journal, and have it match the journal domain name. For example, if your domain name is "hypothesisjournal.com", try using an email address like "editor@hypothesisjournal.com".
* Provide some explanatory text using the "Email Header" setting in Journal Setup Step 1.4. This text will appear at the top of every email generated by the system. Remember, these emails are typically notifications to users, and should be treated just like notification emails from other systems. We recommend the following text: 

```text
You are receiving this email on behalf of <journal-name>. In the event of a requested response, you may respond directly to this email.
```

#### **Solution 2 \(OCS, but also OJS and OMP\):** 

Configure your install to use GMail's SMTP service, as per [this FAQ thread](https://pkp.sfu.ca/wiki/index.php?title=Using_gmail_SMTP).

More here still needs to be written. Will include information on: 

* SMTP stuff
* configuring and troubleshooting



