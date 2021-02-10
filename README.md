# random-api
Simple API returning random status codes
curl https://jw64f0zc20.execute-api.ap-southeast-2.amazonaws.com/prod/hello

# Key features and assumptions
- Deployed in AWS (Sydney region) with Serverless framework using Lambda and API Gateway
- Production stage only
- Manual deployment via GitHub actions
- Monitoring via Serverless dashboard (need a separate organization and team plan to add read-only users)
- Alerting with CloudWatch alerts
- IAM resources pre-configured (roles for local deployment and Serverless dashboard)
- GitHub teams, members and permissions configured
- Scalability and security considerations

# AWS access
- To deploy locally you will need to assume a write role for the account.
  I recommend using aws-vault https://github.com/99designs/aws-vault

# API documentation
You can download documentation from API Gateway
serverless downloadDocumentation --outputFileName=randomapidoc.yml

# Scalability
- API Gateway allows for up to 10,000 requests per second.
- API Gateway and Lambda will scale in response to the traffic spikes
- In real life you might need to implement some rate limiting e.g. define API keys for clients and implement `Usage Plans` and throttling.
- Randomapi is using edge-optimized API endpoint. Requests are routed to the nearest CloudFront Point of Presence.

# Security
- Consider using WAF in front of the API

# Operations
- To monitor deployments and key metrics in CloudWatch (using read-only role for ops team)
- See deployment stats in GitHub actions (read-only access)
- See key metrics (requests count, latency, errors) in CloudWatch
- See alarms in CloudWatch 
- See access logs in CloudWatch logs

# CI/CD
- Using GitHub actions  
- ![Deploy Prod](https://github.com/kiwibel/random-api/workflows/Deploy%20Prod/badge.svg)

# local development

1. Clone the repo
git clone https://github.com/kiwibel/random-api.git

2. Install serverless and required plugins

npm install -g serverless
npm install serverless-plugin-aws-alerts

3. To deploy the app
sls deploy --stage prod

# Further improvements
- Add test job in the pipeline
- Add alert notifications to Slack, PagerDuty etc.
