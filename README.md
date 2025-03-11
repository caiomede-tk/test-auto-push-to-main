# Use Github Action to push directly to main

Usually the default branch `main` is blocked to keep save the code and ensure devs are merging from PRs with the right automated checks. However, sometimes there are some automations we rely a 100% to update our default branch, to be able to keep `main` protected and securely push changes direct to that branch from the execution of a Github Workflow use the following configuration steps:

1. Ensure your repo has a rule already protecting your default branch. Check it trying to push any change directly, or even reviewing the list of rules configured in your repository at Settings --> Rules --> Rulesets.
2. Add a bypass condition to the rule that is blocking pushes to default branch, add the **Deploy keys** there and save.
3. Go and create a Deploy key using a public key of a SSH key your can create locally with `ssh-keygen` command.
4. Configure a secret for your repo using as value the private key of the same SSH key, you can call it `DEPLOY_KEY`.
5. Configure the git checkout step in your workflow to use the deploy key:
     ```yaml
     [...]
           - name: Checkout code
              uses: actions/checkout@v3
              with:
                ssh-key: ${{ secrets.DEPLOY_KEY }}
     [...]
     ```
6. Now test the process of pushing directly to your main branch.

Check the workflow example in this repo for more details.

Solution thanks to community discussion: https://github.com/orgs/community/discussions/25305.
