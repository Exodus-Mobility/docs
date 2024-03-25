# Installation

## Core Requirements:

Install the following base requirements.

1.  [Posgresql](https://www.postgresql.org/)
2.  [NodeJS](https://nodejs.org/en/)
3.  [Terraform](https://www.terraform.io/)
4.  [Python 3.9](https://www.python.org/)
5.  [Virtualenv](https://virtualenv.pypa.io/en/)
6.  Make
7.  [Docker](https://docs.docker.com/)

### Posgresql

=== "Ubuntu"

    Update apt

    ``` sh
    sudo apt update

    ```

    Then, install the Postgres package along with a -contrib package that adds some additional utilities and functionality

    ``` sh
    sudo apt install postgresql postgresql-contrib

    ```

    Press Y when prompted to confirm installation. If you are prompted to restart any services, press ENTER to accept the defaults and continue.


=== "OSX"

    Before you install anything with Homebrew, you should always make sure it’s up to date and that it’s healthy:

    ``` sh
    brew update \
    && brew doctor
    ```

    install postgress

    ``` sh
    brew install postgresql@14
    ```

    start the postgres service

    ``` sh
    brew services start postgresql@14

    ```

    Wait a few seconds, then confirm that it’s running

    ``` sh
    brew services list

    ```


### NodeJS 

=== "Ubuntu"

    Update apt

    ``` sh
    sudo apt update

    ```

    Then install Node.js

    ``` sh
    sudo apt install nodejs

    ```

    Check that the install was successful by querying node for its version number

    ``` sh
    node -v

    ```



=== "OSX"


    Before you install anything with Homebrew, you should always make sure it’s up to date and that it’s healthy:

    ``` sh
    brew update \
    && brew doctor
    ```

    install nodejs

    ``` sh
    brew install node
    ```

    Check that the install was successful by querying node for its version number

    ``` sh
    node -v

    ```


###  Terraform

=== "Ubuntu"

    Ensure that your system is up to date and you have installed the gnupg, software-properties-common, and curl packages installed. You will use these packages to verify HashiCorp's GPG signature and install HashiCorp's Debian package repository.

    ``` sh
    sudo apt-get update && sudo apt-get install -y gnupg software-properties-common

    ```

    Install the HashiCorp GPG key.

    ``` sh
    wget -O- https://apt.releases.hashicorp.com/gpg | \
    gpg --dearmor | \
    sudo tee /usr/share/keyrings/hashicorp-archive-keyring.gpg > /dev/null

    ```

    Verify the key's fingerprint.

    ``` sh
    gpg --no-default-keyring \
    --keyring /usr/share/keyrings/hashicorp-archive-keyring.gpg \
    --fingerprint

    ```

    Add the official HashiCorp repository to your system. The lsb_release -cs command finds the distribution release codename for your current system, such as buster, groovy, or sid.

    ``` sh
    echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] \
    https://apt.releases.hashicorp.com $(lsb_release -cs) main" | \
    sudo tee /etc/apt/sources.list.d/hashicorp.list

    ```

    Download the package information from HashiCorp.

    ``` sh
    sudo apt update

    ```

    Install Terraform from the new repository.

    ``` sh
    sudo apt-get install terraform

    ```

=== "OSX"

    update Homebrew.

    ``` sh
    brew update

    ```

    Install the HashiCorp tap, a repository of all our Homebrew packages.

    ``` sh
    brew tap hashicorp/tap

    ```

    Now, install Terraform with hashicorp/tap/terraform.

    ``` sh
    brew install hashicorp/tap/terraform

    ```


Verify the installation

``` sh
terraform -help
```

### Python 3.9

=== "Ubuntu"

    Update apt.

    ``` sh
    sudo apt update

    ```

    Add the deadsnakes PPA repository to the system

    ``` sh
    sudo add-apt-repository ppa:deadsnakes/ppa

    ```

    install python3.9

    ``` sh
    sudo apt install python3.9 -y

    ```



=== "OSX"

    update Homebrew.

    ``` sh
    brew update

    ```
    Now, install python3.9.

    ``` sh
    brew install python@3.9

    ```

validate python version

``` sh
python3.9 --version

```

### Virtualenv

=== "Ubuntu"

    Update apt.

    ``` sh
    sudo apt update

    ```

    Add the deadsnakes PPA repository to the system

    ``` sh
    sudo add-apt-repository ppa:deadsnakes/ppa

    ```

    install python3.9-venv

    ``` sh
    sudo apt install  -y python3.9-venv python3.9-dev

    ```


=== "OSX"

    update Homebrew.

    ``` sh
    brew update

    ```
    Now, install virtualenv

    ``` sh
    brew install virtualenv

    ```

### Make

=== "Ubuntu"

    Update apt.

    ``` sh
    sudo apt update

    ```

    install make

    ``` sh
    sudo apt install make

    ```


=== "OSX"

    update Homebrew.

    ``` sh
    brew update

    ```
    Now, install Make

    ``` sh
    brew install make

    ```




### Docker

=== "Ubuntu"
    Follow instructions in https://docs.docker.com/engine/install/ubuntu/

=== "OSX"
    Follow instructions in https://docs.docker.com/desktop/install/mac-install/

## Dev Environment Setup

=== "Ubuntu"

    create a virtualenv 
    ```sh
    python3.9 -m venv dw-env
    ```
    activate virtualenv  by running the following
    ```sh
    source dw-env/bin/activate
    ```

    configure the new env with pip

    ```sh
    python -m pip install --upgrade pip==21.0.1
    pip install -q setuptools-rust wheel
    ```
    Clone the repo
    ```sh
    git clone git@github.com:Exodus-Mobility/dw-backend.git
    ```
    Navigate to the core backend folder
    ```sh
    cd dw-backend/src
    ```
    Install the requirements
    ```sh
    pip install  -e .
    ```
    Create the  the test database from the command line by running the script:
    ```sh
    createdb sandbox_test

    ```

    Set up node and install ussd dependencies

    ```sh
    cd ussd && yarn

    ```

    To run core backend locally, using mocked aws (`localstack`):
    1. Export your Tanda test keys
    ```sh
    export TANDA_CLIENT_ID=<the client id>
    export TANDA_CLIENT_SECRET=<the client secret>
    export TANDA_ORG_ID=<the org id>
    ```

    2. start the services using make
    ```sh
    make
    ```
    This will create docker containers with core service and other AWS components provisioned in the `Makefile` and make it available locally.

    3. reach the backend service on `http://localhost:4080`:
    admin dashboad - `/admin`  credentials(`username=admin`, `password=@SuperStrongPassword`)
    backend API - `/api/...`
    IPN hooks   - `/hooks/...`


    To stop the running services:
    ```sh
    make stop-core-backend
    ```

=== "OSX"

    create a virtualenv 
    ```sh
    python -m virtualenv --python=python3.9 dw-env
    ```
    activate virtualenv  by running the following
    ```sh
    source dw-env/bin/activate
    ```

    configure the new env with pip

    ```sh
    python -m pip install --upgrade pip==21.0.1
    pip install -q setuptools-rust wheel
    ```
    Clone the repo
    ```sh
    git clone git@github.com:Exodus-Mobility/dw-backend.git
    ```
    Navigate to the core backend folder
    ```sh
    cd dw-backend/src
    ```
    Install the requirements
    ```sh
    pip install  -e .
    ```
    Create the  the test database from the command line by running the script:
    ```sh
    createdb sandbox_test

    ```

    Set up node and install ussd dependencies

    ```sh
    cd ussd && yarn

    ```

    To run core backend locally, using mocked aws (`localstack`):
    1. Export your Tanda test keys
    ```sh
    export TANDA_CLIENT_ID=<the client id>
    export TANDA_CLIENT_SECRET=<the client secret>
    export TANDA_ORG_ID=<the org id>
    ```

    2. start the services using make
    ```sh
    make
    ```
    This will create docker containers with core service and other AWS components provisioned in the `Makefile` and make it available locally.

    3. reach the backend service on `http://localhost:4080`:
    admin dashboad - `/admin`  credentials(`username=admin`, `password=@SuperStrongPassword`)
    backend API - `/api/...`
    IPN hooks   - `/hooks/...`


    To stop the running services:
    ```sh
    make stop-core-backend
    ```







