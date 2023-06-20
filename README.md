# Instance Cop

Did you accidentally turn on an EC2 instance and forgot to turn it off? Well
this project is for you then.

Instance Cop is a slack bot that you can request to start your EC2 instances. 
Once it is started, it will automatically shutdown in one hour if you do not
respond to any of the three reminders it would send 15, 10, and 2 minutes
before it shuts down.

## Progress

- [ ] Receive a slack command to register an existing ec2 instance. This stores
      the running state of the instance along with the instance id and the user
      who requested this as the owner.
- [ ] Recieve a slack command to start the instance. If the instance is already
      running we do not have to do anything but we need to register the user
      who requested to start the instance as a watcher and notify the owner
      on slack that a new user is using the instance. If the instance is not
      running register an eventbridge event to fire after 45 minutes to notify
      the user that the instance will be terminated.
- [ ] When the reminder event fires, notify the watchers and the owner that the
      instance will be shutdown at the `lease_end` time. The slack notification
      should have buttons to renew the lease for further 30, 60 or 120 minutes.
- [ ] When we receive a message to renew the lease, We should update the
      `lease_end` time for the instance and cancel all the existing events
      under event bridge and schedule new events 
- [ ] A eventbridge rule should stop ec2 instances for which the lease has
      ended.

## Stack

It uses the following.

* Aurora PostgreSQL for the database
* Rust for lambda
* SQLX for database handling
* Slack for all notifications
