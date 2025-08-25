Simple Postfix SMTP TLS relay docker⁠ alpine based image with no local authentication enabled (to be run in a secure LAN).

## ** This is based on the image from juanluisbaptiste/postfix

The following env variables need to be passed to the container:

SMTP_SERVER Server address of the SMTP server to use.
SMTP_PORT (Optional, Default value: 587) Port address of the SMTP server to use.
SMTP_USERNAME (Optional) Username to authenticate with.
SMTP_PASSWORD (Mandatory if SMTP_USERNAME is set) Password of the SMTP user. If SMTP_PASSWORD_FILE is set, not needed.
SERVER_HOSTNAME Server hostname for the Postfix container. Emails will appear to come from the hostname's domain.
The following env variable(s) are optional.

SMTP_HEADER_TAG This will add a header for tracking messages upstream. Helpful for spam filters. Will appear as "RelayTag: ${SMTP_HEADER_TAG}" in the email headers.

SMTP_NETWORKS Setting this will allow you to add additional, comma seperated, subnets to use the relay. Used like -e SMTP_NETWORKS='xxx.xxx.xxx.xxx/xx,xxx.xxx.xxx.xxx/xx'.
NOTE: This option only works when running the container with no network isolation using Docker's host mode⁠, so the container can interact with the Docker host's networks. This means that you will be running a SMTP relay with no authentication in your network. Use this option with caution, and have in mind that it may be removed in the future.

SMTP_PASSWORD_FILE Setting this to a mounted file containing the password, to avoid passwords in env variables. Used like -e SMTP_PASSWORD_FILE=/secrets/smtp_password -v $(pwd)/secrets/:/secrets/

SMTP_USERNAME_FILE Setting this to a mounted file containing the username, to avoid usernames in env variables. Used like -e SMTP_USERNAME_FILE=/secrets/smtp_username -v $(pwd)/secrets/:/secrets/

ALWAYS_ADD_MISSING_HEADERS This is related to the always_add_missing_headers⁠ Postfix option (default: no). If set to yes, Postfix will always add missing headers among From:, To:, Date: or Message-ID:.

OVERWRITE_FROM This will rewrite the from address overwriting it with the specified address for all email being relayed. Example settings: OVERWRITE_FROM=email@company.com⁠ OVERWRITE_FROM="Your Name" email@company.com⁠

DESTINATION This will define a list of domains from which incoming messages will be accepted.

LOG_SUBJECT This will output the subject line of messages in the log.

SMTPUTF8_ENABLE This will enable (default) or disable support for SMTPUTF8. Valid values are no to disable and yes to enable. Not setting this variable will use the postfix default, which is yes.

MESSAGE_SIZE_LIMIT This will change the default limit of 10240000 bytes (10MB).

To use this container from anywhere, the 25 port or the one specified by SMTP_PORT needs to be exposed to the docker host server:
```
docker run -d --name postfix -p "25:25"  \
       -e SMTP_SERVER=smtp.bar.com \
       -e SMTP_USERNAME=foo@bar.com \
       -e SMTP_PASSWORD=XXXXXXXX \
       -e SERVER_HOSTNAME=helpdesk.mycompany.com \
       thiagobruch/smtp_relay
```
