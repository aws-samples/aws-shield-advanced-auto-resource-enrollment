## AWS Shield Advanced Automatic Resource Enrollment

Shield Advanced monitoring provides a solution for customers that will allow enhanced protection of their resources against common threats and attacks.  However, enrollment is not automated and ensuring resources are protected can become an onerous task as the number of resources in use increases.  AWS Firewall Manager can provide this level of automation, but not every customer may wish to enroll in this additional service just for automation.

This pattern provides an automated solution that will respond to resource creation and deletion API events and manage the relevant Shield Advanced protection enrollment.  Additionally, the pattern sets up a schedule to run in which it will validate existing resource and their enrollment status with Shield Advanced monitoring.  To provide control over which resources are enrolled, a custom tag is supported (key "exclude_shield_automation" with a value of "true") so resources with this tag are ignored.  The automation is provided by an SSM Automation runbook that will respond to the relevant event source and run the appropriate process flow.

## Usage

1. Clone the repository and identify the solution template in the `templates/` directory.  This template may be uploaded to S3 for use in the next step but it is not necessary.

2. Use the identified solution template to launch an AWS CloudFormation stack.  The solution is self-contained and has two configuration parameters to provide: a list of regions to monitor for ELB and Elastic IP events and a cron statement to define the schedule the automation will run to periodically validate resources.

3. Monitor new CloudFront, ELB, Elastic IP, Global Accelerator, and Route 53 resource creation and ensure said resources are enrolled into Shield Advanced monitoring.  In the region in which the solution template was deployed:
 * View the EventBridge rules Console page in the relevant region (e.g. https://us-east-1.console.aws.amazon.com/events/home?region=us-east-1#/rules) and ensure the default event bus is selected.  Identify the schedule rule for the solution (the name will contain some or all of "rEventsEventBusEventsRuleSchedule") and ensure existing resources are enrolled after the next scheduled execution.
 * To view the automation execution, navigate to the SSM Automation Console page in the relevant region (e.g. https://us-east-1.console.aws.amazon.com/systems-manager/automation?region=us-east-1).

## Security

See [CONTRIBUTING](CONTRIBUTING.md#security-issue-notifications) for more information.

## License

This library is licensed under the MIT-0 License. See the LICENSE file.

