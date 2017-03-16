# Terraform State of Awareness (TFSOA)

A dashboard that helps centralize and monitor disparate [Terraform remote states](https://www.terraform.io/docs/state/).

<img alt="TFSOA Screenshot" src="https://cloud.githubusercontent.com/assets/26415029/23921750/033ff2b2-08bd-11e7-9d78-632edc2c243b.png">

TFSOA accepts pushes of remote Terraform states in a local database.
The Terraform version, JSON serial, JSON version and entire state are saved. TFSOA uses
this information across a multitude of states to present a single dashboard interface
to view statistics on all Terraform states.


### Setup database

```
bundle exec rake db:environment:set
bundle exec rake db:setup
bundle exec rake db:migrate
```

### Recommended setup notes

Place behind and SSL endpoint like an Nginx, ELB or Haproxy to handle SSL. Terraform states typically contain sensative information.

### Start TFSOA

```bash
rackup
```
This will start a rack server on port 9292

### Add your first Terraform state to tfsoa

#### Unique identifiers for state

* team (team name that owns this tf state)
* product (my companies new product this supports)
* service (service name, or project name)
* environment (dev, stage, prod)


#### Example state push

```bash
curl 127.0.0.1:9292/tfsoa/add_tf_state/team/product/service/environment/ \
  -H "Content-Type: application/json" \
  -X \
  POST -d @.terraform/terraform.tfstate
```

#### Example graph push

```bash
terraform graph \
| curl -d @- http://localhost:9292/tfsoa/add_tf_graph/comms/trulia/someservice/prod/
```

### Development Notes

Current entity-relationship diagram

<img width="1158" alt="screen shot 2017-01-02 at 6 37 01 pm" src="https://cloud.githubusercontent.com/assets/538171/21599015/e581df58-d11a-11e6-817f-3ea81895bd98.png">

### Thank you

Thanks to [Smashing gem](https://github.com/Smashing/smashing) for providing a solid framework for tfsoa to utilize
