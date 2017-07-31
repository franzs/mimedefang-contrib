# `check_against_smtp_server()`

This is an alternative implementation of `md_check_against_smtp_server()` using [`Net::SMTP`](http://search.cpan.org/perldoc/Net::SMTP). It is capable of ESMTP's `STARTTLS` and `AUTH`.

Its signature is compatible to `md_check_against_smtp_server()`. Thus it can be used as a drop-in replacement.

A helper function `parse_smtp_cmd_return()` is needed for `check_against_smtp_server()`. It serves the same purpose as mimedefang's `get_smtp_return_code()`.

Two examples can be found in [`mimedefang-filter.check_against_smtp_server`](mimedefang-filter.check_against_smtp_server): `filter_recipient_starttls()` and `filter_recipient_auth()`.

`filter_recipient_starttls()` needs some additional configuration for the certificate filenames. In sendmail's .mc file put something like:

```
define(`confMILTER_MACROS_ENVFROM', `i, {auth_type}, {auth_authen}, {auth_ssf}, {auth_author}, {mail_mailer}, {mail_host}, {mail_addr}, {client_cert_file}, {client_key_file}')dnl

LOCAL_CONFIG
D{client_cert_file}confCLIENT_CERT
D{client_key_file}confCLIENT_KEY
```

In `mimedefang.conf`

```
MD_EXTRA="-a client_cert_file -a client_key_file"
```

is needed.

Thus `$SendmailMacros{'client_cert_file'}` and `$SendmailMacros{'client_key_file'}` can be referenced in `mimedefang-filter`. Make sure you call `read_commands_file()` before.

Of course both `STARTTLS` and `AUTH` can be used in a single `check_against_smtp_server()` call. This is left as an exercise to the reader...