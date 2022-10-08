# Simple JWT App

This project is a JWT App from Udacity. The app can encode a payload sent to it and returns back an encoded Token and can also decode a token. As a part of the Fullstack Nanodegree, it serves as a practice module for lessons from Course 4: Server Deployment and Containerization by learning and applying skills to implement industry standard ways to containerize and deply web applications that leverage knowledge of Cloud services and Kubernetes, by containerizing and deploying a Flask API to a Kubernetes cluster using Docker, AWS EKS, CodePipeline, and CodeBuild.

All backend code follows [PEP8 style guidelines](https://www.python.org/dev/peps/pep-0008/).


## Getting Started

### Pre-requisites and Local Development 

1. **Python 3.7** - Follow instructions to install the latest version of python for your platform in the [python docs](https://docs.python.org/3/using/unix.html#getting-and-installing-the-latest-version-of-python). You can also download a specific release version from <a href="https://www.python.org/downloads/" target="_blank">here</a>.
Check the current version using:
```bash
#  Mac/Linux/Windows 
python --version
```

2. **Virtual Environment** - We recommend working within a virtual environment whenever using Python for projects. This keeps your dependencies for each project separate and organized. Instructions for setting up a virual environment for your platform can be found in the [python docs](https://packaging.python.org/guides/installing-using-pip-and-virtual-environments/)

3. **Docker Desktop** - Installation instructions for all OSes can be found <a href="https://docs.docker.com/install/" target="_blank">here</a>.

4. **Git**: <a href="https://git-scm.com/downloads" target="_blank">Download and install Git</a> for your system. 

5. **Code editor**: You can <a href="https://code.visualstudio.com/download" target="_blank">download and install VS code</a> here.

6. **AWS Account** - An account is required to create resources needed for the deployment.

7. **Python package manager** - PIP 19.x or higher. PIP is already installed in Python 3 >=3.4 downloaded from python.org . However, you can upgrade to a specific version, say 20.2.3, using the command:

```bash
#  Mac/Linux/Windows Check the current version
pip --version
# Mac/Linux
pip install --upgrade pip==20.2.3
# Windows
python -m pip install --upgrade pip==20.2.3
```

8. **Terminal** - This is required to interact with CLI of the utitlities.
- Mac/Linux users: Can use the default terminal.
- Windows users: GitBash terminal or WSL is recommended 

9. **Command line utilities** - Utilities that are required to deploy on AWS.

  - **AWS CLI:** Installed and configured using the `aws configure` command. Another important configuration is the region. Do not use the `us-east-1` because the cluster creation usually fails in `us-east-1`. Change the default region to `us-east-2`:

  ```bash
  aws configure set region us-east-2  
  ```

  Ensure to create all your resources in a single region. 

  - **EKSCTL:** Installed in your system. Follow the instructions [available here](https://docs.aws.amazon.com/eks/latest/userguide/eksctl.html#installing-eksctl) or [here](https://eksctl.io/introduction/#installation) to download and install `eksctl` utility. 

  - **Kubernetes CLI:** Installed in your system. Installation instructions for kubectl can be found [here](https://kubernetes.io/docs/tasks/tools/install-kubectl/).


#### Install Dependencies

Once your virtual environment is setup and running, install the required dependencies by running:

```bash
pip install -r requirements.txt
```

#### Key Pip Dependencies

- [Flask](http://flask.pocoo.org/) is a lightweight backend microservices framework. Flask is required to handle requests and responses.

### Project Structure

These are the files relevant for the current project:
```bash
.
├── Dockerfile 
├── README.md
├── aws-auth-patch.yml
├── buildspec.yml
├── ci-cd-codepipeline.cfn.yml
├── iam-role-policy.json
├── main.py
├── requirements.txt
├── simple_jwt_api.yml
├── test_main.py
└── trust.json
```

## Running the server

From within the directory first ensure you are working using your created virtual environment.

Each time you open a new terminal session, run:

```bash
export JWT_SECRET=myjwtsecret
export FLASK_APP=main.py
export FLASK_ENV=development
flask run
```

These commands put the application in development and directs our application to use the `main.py` file. Working in development mode shows an interactive debugger in the console and restarts the server whenever changes are made. If running locally on Windows, look for the commands in the [Flask documentation](https://flask.palletsprojects.com/en/1.0.x/tutorial/factory/).

Or alternatively, execute:

```bash
export JWT_SECRET=myjwtsecret
export FLASK_APP=main.py
flask --reload
```
 
 The `--reload` flag will detect file changes and restart the server automatically.

## API Reference

### Getting Started
- Base URL: Currently the app can only be run locally and is not hosted at a base URL. The default backend host address is `127.0.0.1:5000`
- Authentication: Even though the app is about JWT, the app doesn't require authentication of API keys.

The app relies on a secret set as the environment variable `JWT_SECRET` to produce a JWT. The built-in Flask server is adequate for local development, but not production, so you will be using the production-ready [Gunicorn](https://gunicorn.org/) server when deploying the app.

### Endpoints

The Flask app that will be used for this project consists of a simple API with three endpoints:

#### GET /
- This is a simple health check, which returns the response 'Healthy'. 
- Request Arguments: None
- Returns: A string 'Healthy'
- `curl 127.0.0.1:5000`
```
"Healthy"
```

#### POST /
- This is a simple health check, which returns the response 'Healthy'. 
- Request Arguments: None
- Returns: A string 'Healthy'
- `curl -X POST 127.0.0.1:5000`
```
"Healthy"
```

#### POST /auth
- This takes a email and password as json arguments and returns a JWT based on a custom secret.
- Request Arguments: email and password
- Returns: A JSON object with one key named `token` which holds the gaenerated JWT
- `curl --data '{"email":"email@provider.com","password":"password123!"}' --header "Content-Type: application/json" -X POST 127.0.0.1:5000/auth`

```json
{
  "token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJleHAiOjE2NjY0Mjc4MzksIm5iZiI6MTY2NTIxODIzOSwiZW1haWwiOiJlbWFpbEBwcm92aWRlci5jb20ifQ.JURoNdjYkCo7bpBNBCJy_TGFYuCgLFPdvCUr-Gcp_lk"
}
```

Now export this returned token value to an environment variable named `TOKEN`:

```bash
export TOKEN=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJleHAiOjE2NjY0Mjc4MzksIm5iZiI6MTY2NTIxODIzOSwiZW1haWwiOiJlbWFpbEBwcm92aWRlci5jb20ifQ.JURoNdjYkCo7bpBNBCJy_TGFYuCgLFPdvCUr-Gcp_lk
```

#### GET /contents
- This requires a valid JWT, and returns the un-encrpyted contents of that token.
- Request Arguments: None
- Returns: JSON object
- `curl --request GET 'http://127.0.0.1:5000/contents' -H "Authorization: Bearer ${TOKEN}"`

```json
{
  "email": "email@provider.com", 
  "exp": 1666430931, 
  "nbf": 1665221331
}
```

## Testing

To deploy the tests, run

```bash
python test_main.py
```

## Dockerizing

The app can be containerized usaiang Docker and can be tested before deployment.

1. Containerize the app

```bash
docker build -t simple-jwt-api .
```

Alternateively, you can pull this docker containeized image from my repository by running:

```bash
docker pull kidusmik/simple-jwt-api:latest
```

2. Run the container

```bash
docker run --name myContainer --env-file=.env -p 80:8080 simple-jwt-api
```

Notice that the app will be running on `127.0.0.1:80`

## Deployment

This app is deployed in AWS on Kubernetes, the steps followed to deploy the app are:

1. **Create a cluster**

```bash
eksctl create cluster --name simple-jwt-api --nodes=2 --version=1.23 --instance-types=t2.medium --region=us-east-2
```

This will return:

```
2022-10-04 14:28:39 [✔]  saved kubeconfig as "C:\\Users\\kidusmik\\.kube\\config"
2022-10-04 14:28:39 [✔]  all EKS cluster resources for "simple-jwt-api" have been created
2022-10-04 14:28:44 [ℹ]  nodegroup "ng-3172ae09" has 2 node(s)
2022-10-04 14:28:44 [ℹ]  node "ip-192-168-25-243.us-east-2.compute.internal" is ready
2022-10-04 14:28:44 [ℹ]  node "ip-192-168-66-129.us-east-2.compute.internal" is ready
2022-10-04 14:30:38 [ℹ]  kubectl command should work with "C:\\Users\\kidusmik\\.kube\\config", try 'kubectl get nodes'
2022-10-04 14:30:38 [✔]  EKS cluster "simple-jwt-api" in "us-east-2" region is ready
```

2. **Check the status of the Nodes**

```bash
kubectl get nodes
```

This will return:

```
NAME                                           STATUS   ROLES    AGE     VERSION
ip-192-168-25-243.us-east-2.compute.internal   Ready    <none>   6m39s   v1.23.9-eks-ba74326
ip-192-168-66-129.us-east-2.compute.internal   Ready    <none>   6m45s   v1.23.9-eks-ba74326
```

3. **Fetch the caller identity**

```bash
aws sts get-caller-identity --query Account --output text
```

This will return:
```
026423306961
```

4. **Create a role for Kubectl to assume**

```bash
aws iam create-role --role-name UdacityFlaskDeployCBKubectlRole --assume-role-policy-document file://trust.json --output text --query 'Role.Arn'
```

This will return:
```
arn:aws:iam::026423306961:role/UdacityFlaskDeployCBKubectlRole
```

5. **Set the JWT_SECRET environment variable**

```bash
aws ssm put-parameter --name JWT_SECRET --overwrite --value "myjwtsecret" --type SecureString
```

This will return:

```json
{
    "Version": 1,
    "Tier": "Standard"
}
```

6. **Check if the JWT_SECRET environment variable is set**

```bash
aws ssm get-parameter --name JWT_SECRET
```

This will return:

```json
{
    "Parameter": {
        "Name": "JWT_SECRET",
        "Type": "SecureString",
        "Value": "AQICAHiTukfjMafjbLZBfg2XSgOFG3p+oe3/mz6+ROusuat+BgHtrs8M4kaJjAy9usqZYPpBAAAAaTBnBgkqhkiG9w0BBwagWjBYAgEAMFMGCSqGSIb3DQEHATAeBglghkgBZQMEAS4wEQQMqN9qfGD/WDRTewkPAgEQgCZByo37bvXlsUaG/l4WOqkAn/sVKCl99ClTdcjZi7Ll0ub1SQ9gkA==",
        "Version": 1,
        "LastModifiedDate": "2022-10-04T16:21:24.707000+03:00",
        "ARN": "arn:aws:ssm:us-east-2:026423306961:parameter/JWT_SECRET",
        "DataType": "text"
    }
}
```

7. **Get the external IP address or URL**

```bash
kubectl get services simple-jwt-api -o wide
```

This will return:

```
NAME             TYPE           CLUSTER-IP       EXTERNAL-IP                                                               PORT(S)        AGE   SELECTOR
simple-jwt-api   LoadBalancer   10.100.176.163   a4dfaec9cd70246a787306736f2e642c-1823854276.us-east-2.elb.amazonaws.com   80:32611/TCP   12m   app=simple-jwt-api
```

## Authors
The Udacity Team and yours truly, Kidus Worku


## Acknowledgements 
The awesome team at Udacity and the ALX-T community.
