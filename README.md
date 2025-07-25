Trufflrhog:

Instalation:
Local:
brew tap trufflesecurity/trufflehog
brew install trufflehog

Docer:
docker run --rm -it -v "$PWD:/pwd" trufflesecurity/trufflehog:latest github --org=trufflesecurity


Run:

git:
trufflehog git https://github.com/KarolodonerPL/trufflehog-tests.git --results=verified,unknown

local:
trufflehog filesystem ./PycharmProjects/trufflehog-tests  --json > output.json

Output:
json, cvs

Possible use with pytest?

Test Case:
 
 1) When: Scan empty repo                     Then: No Secret found
 2) When: Scan Repo with secrets              Then: Secrates are detected
 3) When: scan with flag  --only-verified     Then: Verified secrets present in report
 4) When: Scan with  json file output         Then: file with json respons present
 5) When: Scan git repo online                Then: Secrets in repo history are found if exist
 5) When: run on non existing folder          Then: Error is present - no such folder
 

Strategy to test bulk scans
1) Create a scan job (for example in Jenkins or bitbucket)
2) Get list of all repo - or have it created?
3) Run TruffleHog  for each repo
4) Send Jsons to db
5) Check db storage: schema should contain: Repo, location, verified
6) Send email to selected person when any problem find and add info in run
7) Performance testing: check bulk scan on big repos and big repos number
8) Compare scan to previes one

