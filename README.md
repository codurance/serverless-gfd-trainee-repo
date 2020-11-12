Instructions
============
- Clone the following [repository](https://github.com/codurance/serverless-base) which will give you access to a fully aws-serverless enabled container
- Clone the following [repository](https://github.com/codurance/serverless-gfd-trainee-repo). This will be the frontend project
- Add the frontend project as a volume to your container. If the project is called `lambda-training`, then you can add the volume like this
  - Edit `docker-compose.yml`file and add
  - ```
    ...
    ./serverless:/home/svrless
    $PATH_TO_YOUR_FOLDER/lambda-training:/home/lambda-training
  - /Users/YOURNAME/Dev/bench/serverless/frontend:/home/lambda-training/frontend
    ...
    ``` 
- Access your docker container by running
  - `cd YOUR_AWS_SERVERLESS_CONTAINER_REPO`
  - `docker-compose build`
  - `docker-compose up -d `
  - `docker exec -it serverless-image_serverless-framework_1 /bin/bash` Where `serverless-image_serverless-framework_1` is the name of your container. You can get the name by running `docker-compose ps`
  Once there, you'll get something like this
  ```
  root@ec5fe463cbb6:/home/svrless#
  ```
  Now you have access to your container. You can move to `/home/lambda-training` and perform everything from that folder
## 1 Creating your first lambda service
In order to create your first lambda service, you need to 
    
- install the aws sdk and configure it
- install the library middy
- install serverless-offline

- create a hello world lambda serverless create --template aws-nodejs
- invoke it locally 

## 2 Create a lambda that have cors

- Use middy to have it working,  
- User curl -v to see the headers 

# 3 Create a dynamo db database and give permissions for it

Tip:

  ```iamRoleStatements:
    - Effect: Allow
      Action:
        - dynamodb:DescribeTable
        - dynamodb:Query
        - dynamodb:Scan
        - dynamodb:GetItem
        - dynamodb:PutItem
        - dynamodb:UpdateItem
        - dynamodb:DeleteItem
      # Restrict our IAM role permissions to
      # the specific table for the stage
      Resource:
        - "arn:aws:dynamodb:us-east-1:300563897675:table/serverless-gfd-it1-posts-database"
```

```
aws dynamodb put-item \
    --table-name serverless-gfd-it1-posts-database \
    --item '{
      "id": {"S": "200300400"}, "posts": { "S": "CONGRATULATIONS !data_from_the_database"}
    }'
```

# 4 Format the output to be a plain json

Tip:
we have to convert from dynamodb format...



# 5 Connect the database to the frontend

post this well formatted data on the DB

```
aws dynamodb put-item \
    --table-name serverless-gfd-it1-posts-database \
    --item '{
      "id": {"S": "1234"}, "posts": { "L": [    { "M" : {        "dateTime" : { "N" : "1549312452000" },        "postId" : { "S" : "102030" },        "text" : { "S" : "Fake post done via CLI while backend is under construction" },        "userId" : { "S" : "1234" }      }    },    { "M" : {        "dateTime" : { "N" : "1549312453000" },        "postId" : { "S" : "102031" },        "text" : { "S" : "Fake post from another user while backend is under construction" },        "userId" : { "S" : "1234" }      }    }  ] }
    }'
```

lambda should output the posts array 


# 6 Connect frontend locally

to run the frontend locally you need: `yarn install && yarn start-insecure` passing the endpoint acording to the Readme of the frontend project.


Tip: you will need serverless running local. If you're running from docker image, consider the exposed ports as a hint if you have collisions. Also, when running serverless local, add --host 0.0.0.0, in that way, your service will be visible everywhere in your network


# 7 Connect frontend with deployed Lambda in AWS

First you need to deploy your service and check if it's possible to call it. After that, run your frontend passing the remote endpoint to `yarn start-insecure`

# 8 Deploy to s3 bucket

At this point, it's necessary to generate a "production build" for the frontend app, take the generated sources and copy them to S3. Don't forget to set the endpoint

You need to run `deploy_frontend.sh` but without forgetting to pass the endpoint. You might need to modify the script in that case
