# Adding A new Service

To add a new service to the mono-repo(dw-backend) 

1. Add a new folder at the root of this repo with the name of your new service

2. In the folder you created add your source files on the language you are developing i.e python, java, js etc

3. Ensure you write test for your service and your test can be run by a test runner e.g nosetets/pytest if python etc

4. Update the CI to include test for your new service by editing the [git hub action file of our CI ci.yml](https://github.com/Exodus-Mobility/dw-backend/blob/main/.github/workflows/ci.yml)

    ``` yml linenums="62"

    ....
    - name: Test <The name of my service>
            
            run: |
            pushd <my new service folder name>
            <commands to run the test of my service>
            popd

    ```

5. On `/terraform` folder add the terraform definitions of the infrastructure that your new service will be using i.e database definitions, lambda definitions etc basically any aws resource that you are adding for this new service. Name it to match the name of your service.

6. Update the CD to include deployment of your new service by editing the [git hub action file of our CD deploy.aws.yml
](https://github.com/Exodus-Mobility/dw-backend/blob/main/.github/workflows/deploy.aws.yml) using the dorny/paths-filter@v2 add the file paths to your new service to be watched so that if any of them would change the CD will kick off your deploy step.

7. Update the readME with the information about your new service such as how to run it, test it etc.