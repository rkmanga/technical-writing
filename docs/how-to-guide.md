<section class="section" markdown="1">

<h2 class="section-title"><span class="icon-holder"><i class="fas fa-tasks"></i> How To Guide</span></h2>

<div class="annotation-block" style="background: #11-0602; border: 0px solid #c3ddd5; border-radius: 4px; padding: 18px 22px; margin-bottom: 32px;">
  <h4 style="margin-top: 0; color: #02ffff; font-size: .75rem;">About This Sample</h4>
  <table style="width: 100%; font-size: 0.88rem; border-collapse: collapse;">
    <tr>
      <td style="width: 130px; font-weight: 500; padding: 4px 10px 4px 8px; vertical-align: top; color: #11-0602;">Product</td>
      <td style="padding: 4px 0; color: #11-0602;">GitHub Actions — a real, widely used CI/CD automation platform built into GitHub repositories, available free for public repos.</td>
    </tr>
    <tr>
      <td style="font-weight: 500; padding: 4px 10px 4px 8px; vertical-align: top; color: #11-0602;">Audience</td>
      <td style="padding: 4px 0; color: #11-0602;">Junior-to-mid-level developers who write code and push to GitHub regularly but have not set up automated testing or deployment pipelines before.</td>
    </tr>
    <tr>
      <td style="font-weight: 500; padding: 4px 10px 4px 8px; vertical-align: top; color: #11-0602;">Tools Used</td>
      <td style="padding: 4px 0; color: #11-0602;">Markdown, YAML, Git, VS Code, GitHub.com workflow editor.</td>
    </tr>
    <tr>
      <td style="font-weight: 500; padding: 4px 10px 4px 8px; vertical-align: top; color: #11-0602;">Challenge Solved</td>
      <td style="padding: 4px 0; color: #11-0602;">GitHub Actions documentation is written for users who already understand CI/CD concepts. The challenge was demystifying workflow syntax — triggers, jobs, steps, runners — and connecting abstract concepts to a concrete, runnable example a developer could have working in under 30 minutes.</td>
    </tr>
  </table>
</div>

---

## How To Set Up a CI/CD Pipeline for a Node.js App Using GitHub Actions

This guide walks you through creating an automated pipeline that runs your tests and reports results on every push to GitHub. By the end, every commit you push will automatically trigger your test suite — and you'll see pass/fail status directly on your pull requests.

**Time to complete:** ~20 minutes

---

### Prerequisites

Before you begin, confirm the following:

- You have a GitHub account and a repository with a Node.js project.
- Your project has a `package.json` with a `test` script (e.g., `"test": "jest"` or `"test": "npm run mocha"`).
- You have at least one test file that can be run with `npm test`.
- You have write access to the repository.

!!! tip
    **Don't have tests yet?**
    
    Add a placeholder: create `test/sample.test.js` with `test('placeholder', () => { expect(1).toBe(1);});` 
    and add `"test": "jest"` to your `package.json` scripts. Install Jest with `npm install --save-dev jest`.

---

### Step 1: Create the Workflow Directory

GitHub Actions looks for workflow files in a specific directory. In your project root, create the following folder structure:

```
your-repo/
└── .github/
    └── workflows/
```

You can create this in the terminal:

```bash
mkdir -p .github/workflows
```

Or create it directly in the GitHub web editor by navigating to your repo, clicking **Add file > Create new file**, and typing `.github/workflows/` as the beginning of the file path.

---

### Step 2: Create the Workflow File

Inside `.github/workflows/`, create a new file called `ci.yml`. This file defines your pipeline.

Copy and paste the following workflow:

```yaml
name: Node.js CI

on:
  push:
    branches: [ "main" ]
  pull_request:
    branches: [ "main" ]

jobs:
  test:
    runs-on: ubuntu-latest

    strategy:
      matrix:
        node-version: [18.x, 20.x]

    steps:
      - name: Check out code
        uses: actions/checkout@v4

      - name: Set up Node.js ${{ matrix.node-version }}
        uses: actions/setup-node@v4
        with:
          node-version: ${{ matrix.node-version }}
          cache: 'npm'

      - name: Install dependencies
        run: npm ci

      - name: Run tests
        run: npm test
```

---

### Understanding the Workflow File

| Section | What it does |
|---------|-------------|
| `name` | The display name shown in the GitHub Actions tab |
| `on: push` | Triggers the workflow when you push to `main` |
| `on: pull_request` | Also triggers on pull requests targeting `main` |
| `jobs.test` | Defines a job called `test` |
| `runs-on: ubuntu-latest` | Uses a GitHub-hosted Ubuntu runner (free for public repos) |
| `matrix.node-version` | Runs the job twice — once with Node 18, once with Node 20 |
| `actions/checkout@v4` | Checks out your repository code onto the runner |
| `actions/setup-node@v4` | Installs the specified Node.js version |
| `npm ci` | Installs dependencies from `package-lock.json` (faster and more consistent than `npm install`) |
| `npm test` | Runs your test script |

---

### Step 3: Commit and Push the Workflow File

Save your `ci.yml` file and commit it to your repository:

```bash
git add .github/workflows/ci.yml
git commit -m "Add GitHub Actions CI workflow"
git push origin main
```

---

### Step 4: Verify the Pipeline Ran

1. Go to your repository on GitHub.
2. Click the **Actions** tab. You should see your workflow listed under the commit you just pushed.
3. Click the workflow run to see the job details, including the output of each step.
4. A green checkmark indicates all steps passed. A red ✗ means a step failed — click the step name to see the full log output.

---

### Step 5: Check Status on a Pull Request

Create a new branch, make a small change, and open a pull request. After pushing, scroll to the bottom of the PR page. You should see a **checks section** showing the status of your workflow. GitHub blocks merging if you configure branch protection rules (optional, covered in [Protecting the main branch](#protecting-the-main-branch-optional)).

---

### Protecting the Main Branch (Optional)

To prevent merging code that fails tests, enable branch protection:

1. Go to **Settings > Branches** in your repository.
2. Under **Branch protection rules**, click **Add rule**.
3. Set the **Branch name pattern** to `main`.
4. Check **Require status checks to pass before merging**.
5. Search for and select your `test` job from the list.
6. Click **Save changes**.

Now GitHub will block pull request merges if the CI tests fail.

---

### Troubleshooting

**Workflow not appearing under Actions?**
Ensure the file is in `.github/workflows/` (not `.github/workflow/`) and is a valid `.yml` file.

**`npm ci` fails with "missing package-lock.json"?**
Run `npm install` locally to generate a `package-lock.json`, then commit it.

**Tests pass locally but fail in CI?**
Check for environment-specific values (e.g., hard-coded localhost URLs or missing environment variables). Add secrets via **Settings > Secrets and variables > Actions**.

---

*This guide was written and verified against a Node.js 20 project using the GitHub Actions web editor and VS Code.*

</section>
