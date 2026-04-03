## Important things

- For AWS oriented organisation , we will only use AWS related things like AWS CLI , AWS CFT , AWS CDK for automating the infrastructure (Creation of Virtual Machine )
- We could mobaxterm(Windows) , nomachine(w +Mac) or iTerm(mac) for login in the EC2 instance through terminal.
- A good practice is to always stop the instance first and then delete it.

## What I did
- I created virtualfile by connecting from EC2 instance virtually.
- I logined thorugh ssh session into my EC2 ubuntu instance with the help of key-value pair which was generated during the creation of VM. 
- And then we crossed check the virtulfile and it exist and we login to the same VM and then we checked through ls
- And created another file : touch localfile , Suggesting that both file created at different places through different means in same VM doesn't affect it.
