# Git Workflow
All repositories should follow git workflow as explained below.

### Branches
Every repository must have at least three branches that will be present through out the lifecycle of the product.
1. **master** - the stable branch. Code from its HEAD is running on production environment.
2. **pre-master** - the branch containing the next set of features/changes that are to go out in the next production deployment. This is the branch that User Acceptance Testing must be performed on.
3. **dev** - the branch that contains the latest and greatest features that are not completely ready for production.

### Environments,
* Have a minimum of two environments, production & staging. In the case of supertax we have a third environment, development.
* Staging is used for UAT & demos and development is used for dev integration.
* Each environment should at any given time be running code from only branch.

### Workflows

#### Feature Development
* Feature development **must** be performed **only** on feature branches.
* Feature branches **must** be created off of the **dev** branch and **must** be merged back into the dev branch.
* Create a feature branch using,
```
git checkout -t -b my-feature-branch origin/dev
git push -u origin my-feature-branch 
```
* Once you have completed your feature work and you think the feature is ready for integration testing you can merge your feature branch into dev by first merging the latest changes from dev into your branch,
```
git checkout dev
git pull origin dev
git checkout my-feature-branch
git pull origin dev
git push origin my-feature-branch
```
* Then by merging your changes into dev using either the command line as,
```
git checkout dev
git merge --no-ff my-feature-branch
git branch -d my-feature-branch
git push origin dev
```
* You can also merge your changes into dev by raising a PR of your branch against dev.

#### UAT
* Once dev is ready to be moved to UAT a PR **must** be created for merging dev into **pre-master**.
* **pre-master** is what is deployed on the UAT/Staging environment and must be thoroughly tested.
* If a bug is detected in pre-master that needs to be fixed in pre-master, maybe because of severity or because dev has now moved ahead. The following steps must be followed.
	* Create a **uat-fix** branch from **pre-master**.
	* Make the necessary changes on the **uat-fix** branch.
	* Once the **uat-fix** branch is ready, merge it back into **pre-master** and **dev**.
	* Merging of the **uat-fix** branch into dev is extremely important since merging between **pre-master** and **dev** is always uni-directional.

#### Production
* Once the pre-master branch has passed UAT and regression testing, a PR must be created for merging **pre-master** into **master**.
* **master** should then be deployed onto the production environment and then tagged with a **release-** tag.
* If a bug is detected in production that needs to be fixed in **master**. The following steps **MUST** be followed,
	* Create a **hotfix-** branch from **master**.
	* Make the necessary changes on the **hotfix-** branch and test them on the UAT/Staging environment.
	* Once the **hotfix-** branch is ready, merge it into **master**.
	* The **hotfix-** branch **MUST** also be merged into **dev** and optionally into **pre-master**.

#### Why pre-master ?
* **pre-master** branch is just a gateway to master and it’s main purpose is to prevent any mistaken merges from dev into master.
* It serves as a layer of protection of the pristine/stable master branch.

#### References
* [A successful Git branching model » nvie.com](http://nvie.com/posts/a-successful-git-branching-model/)

