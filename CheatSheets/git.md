---
id: git
aliases:
  - Git Flow
tags: []
---

`git init` 
`git status`
`git add file`
`git commit`
`git remote add git@github:juangriffin121/reponame.git`
if i put it with https it will ask me for password or PAT which is trash with the new url its better with ssh and if i have it done i can just push, if i had the https one i can change it like this
`git remote set url git@github:juangriffin121/reponame.git`
i can see if its the previous one like this:
`git remote -v`
`git push -u origin main|master`

# Git Flow

A good workflow is to use a branching model that separates stable code from active development. One common approach is similar to Git Flow:

1. **Main (or Master) Branch:**
   - **Purpose:** Always holds the stable, production-ready code.
   - **Usage:** Releases are tagged on this branch.

2. **Develop Branch:**
   - **Purpose:** Integrates all new features and fixes.
   - **Usage:** Feature branches are merged into develop. This branch is regularly tested, and when a set of features is ready, a release branch is created from develop.

3. **Feature Branches:**
   - **Purpose:** Each new feature is developed on its own branch.
   - **Naming Convention:** Use descriptive names like `feature/add-login` or `feature/new-report`.
   - **Usage:** When a feature is complete and tested locally, open a pull request to merge it into develop.

4. **Release Branch:**
   - **Purpose:** Prepare for a new release.
   - **Usage:** Once develop has accumulated a set of features and bug fixes, create a release branch (e.g., `release/v1.1.0`), update version numbers and changelog, perform final testing and bug fixes on this branch.
   - **Final Steps:** Merge the release branch into both main and develop, and then tag the new release on main.

5. **Hotfix Branches (Optional):**
   - **Purpose:** Quickly address production issues.
   - **Usage:** Create a hotfix branch from main, fix the issue, then merge the hotfix back into main and develop (or release, if needed) and tag a new version.

**Example Workflow:**

- **Step 1:** Start with your stable main branch.
- **Step 2:** Create a develop branch from main.
- **Step 3:** For each new feature, create a feature branch off develop.
- **Step 4:** When a feature is ready, open a pull request to merge it into develop.
- **Step 5:** Once multiple features are integrated and tested in develop, create a release branch (e.g., `release/v1.1.0`).
- **Step 6:** Update version numbers, changelog, and do final testing on the release branch.
- **Step 7:** Merge the release branch into main, tag the new release, and merge it back into develop.
- **Step 8:** Deploy from the tagged version on main.

This keeps your main branch stable, makes code reviews easier via pull requests on feature branches, and helps you track and release new versions systematically. Use CI/CD to run tests on all branches, and follow semantic versioning for your releases.
