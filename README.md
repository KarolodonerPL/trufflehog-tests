Trufflrhog:

Instalation:
Local:
brew tap trufflesecurity/trufflehog
brew install trufflehog

or using go:
git clone https://github.com/trufflesecurity/trufflehog.git
cd trufflehog; go install

Docer:
docker run --rm -it -v "$PWD:/pwd" trufflesecurity/trufflehog:latest github --org=trufflesecurity

Verifying the artifacts:
You need the following tool to verify signature:
Cosign
https://docs.sigstore.dev/cosign/system_config/installation/

Run:

git:
trufflehog git https://github.com/KarolodonerPL/trufflehog-tests.git --results=verified,unknown

local:
trufflehog filesystem ./PycharmProjects/trufflehog-tests  --json > output.json

Output:
json, cvs

Possible use with pytest?

Test Case:
 
 1) Test Case 01: Scan local empty repo
    Given: repo does not contain any secrets  When: Scan empty repo    Then: No Secret found
    "trufflehog filesystem ../PycharmProjects/pythonProject"

 2) Test Case 02: Scan local repo with secrets
    Given: Local repo contain secrets   When: Scan Repo with secrets  Then: Secrets are detected
    "trufflehog filesystem ../PycharmProjects/trufflehog-tests"

 3) Test Case 03: Scan local repo with secrets and create json
    Given: Local repo contain secrets   When: Scan Repo with secrets and create json  
    Then: Secrets are detected and json file is created
    "trufflehog filesystem ../PycharmProjects/trufflehog-tests --json  > output.json"

 4) Test Case 04: Scan local repo with secret with flag: verified
    Given: Local repo contain secrets   When: Scan Repo with flag verified
    Then: Verified Secrets are detected 
    "trufflehog filesystem ../PycharmProjects/trufflehog-tests  --results=verified,unknown  --json  > output.json"

 5) Test Case 05: Scan local file with secret with flag: verified
    Given: Local file contain secrets   When: Scan Repo with flag verified Then: Verified Secrets are detected
    "trufflehog filesystem ../PycharmProjects/trufflehog-tests/config.env  --results=verified,unknown  --json  > output.json"

 6) Test Case 06: Scan online repo with secret  with flag: verified
    Given: online repo with secrets exist  When: Scan git repo online Then: Secrets in repo history are found if exist
    "trufflehog git https://github.com/KarolodonerPL/trufflehog-tests.git --results=verified,unknown --json > output.json"

 7) Test Case 07: Scan local non-existing repo with flag: verified
    When: run on non-existing folder          Then: Error is present - no such folder
    "trufflehog filesystem ../PycharmProjects/trufflehog-tests_not_exiting --json  > output.jso"

TODO
 8) Test Case 08: Scan repo using docker
    Given: Online repo contain secrets and docker is running When: Scan Repo  Then: Verified Secrets are detected
 

Strategy to test bulk scans
1) Create a scan job (for example in Jenkins or bitbucket)
2) Get list of all repo - or have it created?
3) Run TruffleHog  for each repo
4) Due to performance parallel testing can be use
5) Send Jsons to db
6) Check db storage: schema should contain: Repo, location, verified
7) Send email to selected person when any problem find and add info in run
8) Performance testing: check bulk scan on big repos and big repos number
9) Compare scan to previous one

