Instructions
============
- Clone the following [repository](https://github.com/codurance/serverless-gfd-trainee-repo). This will be the frontend project

## Goal 1: have a lambda doing a hello world - 1h
In order to create your first lambda service, you need 

   brew install awscli
   npm init
   npm install -g serverless
   npm install middy
   npm install serverless-offline

create a hello world lambda, deploy it, call it 

#### quick fix -- ask  - 30m

![](https://raw.githubusercontent.com/codurance/serverless-gfd-trainee-repo/_master/docs/middy.png)

- Use middy to have it working,  
- User curl -v to see the headers 

## Goal 2: allow the Lambda to read from a database - 1h30m

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

Have the lambda returning this database entry 
```
var ddb = new AWS.DynamoDB({ apiVersion: '2012-08-10' });
ddb.getItem(...
```

## Goal 3: have the output of the dynamodb to have a json shape - 1h

Tip:
we have to convert from dynamodb format...



## Goal 4: Connect the lambda to the frontend - 1h

post this well formatted data on the DB

```
aws dynamodb put-item \
    --table-name serverless-gfd-it1-posts-database \
    --item '{
      "id": {"S": "1234"}, "posts": { "L": [    { "M" : {        "dateTime" : { "N" : "1549312452000" },        "postId" : { "S" : "102030" },        "text" : { "S" : "Fake post done via CLI while backend is under construction" },        "userId" : { "S" : "1234" }      }    },    { "M" : {        "dateTime" : { "N" : "1549312453000" },        "postId" : { "S" : "102031" },        "text" : { "S" : "Fake post from another user while backend is under construction" },        "userId" : { "S" : "1234" }      }    }  ] }
    }'
```

to run the frontend locally you need: `yarn install && yarn start-insecure` passing the endpoint acording to the Readme of the frontend project.

Follow the following tips

![](https://raw.githubusercontent.com/codurance/serverless-gfd-trainee-repo/_master/docs/connect-frontend-tips.png)
![](https://raw.githubusercontent.com/codurance/serverless-gfd-trainee-repo/_master/docs/connect-frontend-tips-2.png)

