# Secret scan pipeline
This pipeline will take the repo from git in our case bit bucket, and than scan from the secrets, if they are found it will make red build, than it will sent it to JFROG Artifactory

## This pipeline will
1.	Check out code from Bitbucket.
2.	Scan the code for secrets using Gitleaks.
3.	Build the code (using Maven in this example).
4.	Upload the built artifact to JFrog Artifactory.

## Setup:
Ensure Gitleaks is installed on the Jenkins agent where the pipeline will run. You can install it using the following commands:
```bash
curl -sSL https://github.com/gitleaks/gitleaks/releases/latest/download/gitleaks-linux-amd64 -o /usr/local/bin/gitleaks
chmod +x /usr/local/bin/gitleaks
```
