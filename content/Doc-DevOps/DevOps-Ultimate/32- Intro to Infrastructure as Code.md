It is developed by Hashicorp.
Terraform manage through providers 
A provider helps TerraForm manage third party platforms through their ***API***

| **Infrastructure Platform** | **Networking** | **Monitor**    | **Database**   | **Control version** |
| ----------------------- | ---------- | ---------- | ---------- | --------------- |
| Physical  Machines      | BigIP      | DataDog    | influxDB   | GitHub          |
| VMWare                  | CloudFlare | Grafana    | MongoDb    | BitBucket       |
| AWS                     | DNS        | Auth0      | MySQL      | GitLab          |
| GCP                     | Palo Alto  | Wavefront  | PostgreSQL |                 |
| Azure                   | Infoblox   | Sumo Logic | VCS        |                 |
TerraForm uses SQL.
***What is a declarative code?***
- The code we define is the state that we want our infrastructure to be in.
- TerraForm works in three pahses:
	- INIT: Initializes the project and identifies the providers to be used for hte target environment
	- PLAN: Drafts a plan to get to the target state.
	- APPLY: Makes the necessary changes required on the target environment to bring it to the desired.

The state is a blueprint of the infrastructure deployed by TerraForm.