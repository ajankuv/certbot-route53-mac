certbot-route53 for osx
===============

**NOTE: This is only for OSX the linux version can be found [Here][].**

This shell script helps create [Let's Encrypt][] certificates for [AWS Route53][]. It uses [Certbot][] to automate certificate requests, and the [AWS CLI][] to automate DNS challenge record creation.

Installation and Usage
----------------------
1. Ensure you have the updated sed from [Homebrew][] (`brew install gnu-sed`)

2. Install Certbot and the AWS CLI. You can use [Homebrew][] (`brew install awscli certbot`) or [pip][] (`pip install awscli certbot`).

3. [Configure the AWS CLI][]. Your account must have permission to list and update Route53 records.

4. Download the [certbot-route53.sh][] script.

    ```sh
    mkdir my-certificates
    cd my-certificates
    curl -sL https://git.io/fpMeL -o certbot-route53.sh
    chmod a+x certbot-route53.sh
    ```

5. Run the script with your (comma-separated) domain(s) and email address:

    ```sh
    sh certbot-route53.sh \
      --agree-tos \
      --manual-public-ip-logging-ok \
      --domains *.jedicloudsolutions.com \
      --email $(git config user.email)
    ```

6. Wait patiently (usually about two minutes) while, for each domain requested:

    - Certbot asks Let's Encrypt for a DNS validation challenge string,
    - AWS CLI asks Route53 to create a domain TXT record with the challenge value,
    - Let's Encrypt validates the TXT record and returns a certificate, and finally
    - AWS CLI asks Route53 to delete the TXT record.

7. Find your new certificate(s) in the `letsencrypt/live` directory.

![terminal](https://cloud.githubusercontent.com/assets/4433/23584470/9306b8ac-0130-11e7-9ffc-ef7d91971620.png)

[AWS Route53]: https://aws.amazon.com/route53
[Let's Encrypt]: https://letsencrypt.org
[Certbot]: https://certbot.eff.org
[AWS CLI]: https://aws.amazon.com/cli/
[Homebrew]: https://brew.sh/
[pip]: https://pypi.python.org/pypi/pip
[certbot-route53.sh]: https://git.io/fpMeL
[Configure the AWS CLI]: http://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html
[Here]: https://github.com/jed/certbot-route53
