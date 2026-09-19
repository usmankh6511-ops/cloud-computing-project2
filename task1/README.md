# GardCom Developer Portfolio — Static Cloud Hosting Project

**Project 1: The Global Launch** — DecodeLabs Cloud Computing Internship (AWS/Azure)

A personal portfolio website hosted entirely on object storage (no traditional server), demonstrating serverless static hosting on AWS S3 or Azure Blob Storage.

## What's in this folder

```
portfolio-project/
├── index.html          # main portfolio page
├── 404.html             # custom error page
├── bucket-policy.json   # AWS public-read policy (edit before use)
└── README.md            # this file
```

## 0. Before you start — edit your details

Open `index.html` and replace the placeholder links with your real ones:
- `you@example.com`
- `github.com/yourusername`
- `linkedin.com/in/yourusername`

Preview it locally first: just double-click `index.html` to open it in your browser and check everything looks right.

---

## 1. GitHub Repository Setup

```bash
# from inside the portfolio-project folder
git init
git add .
git commit -m "Initial commit: portfolio site for cloud hosting project"

# create the repo on GitHub (via gh CLI) — or create it manually on github.com and skip this line
gh repo create your-portfolio-cloud-hosting --public --source=. --remote=origin

# push
git branch -M main
git push -u origin main
```

If you don't have the GitHub CLI (`gh`), just create an empty **public** repository on github.com, then run:
```bash
git remote add origin https://github.com/YOUR-USERNAME/YOUR-REPO-NAME.git
git branch -M main
git push -u origin main
```

Checklist reminder: make sure the repo is set to **Public** before submission.

---

## 2. Option A — AWS S3 Deployment

### Prerequisites
Install and configure the AWS CLI first:
```bash
aws configure
# enter your Access Key ID, Secret Access Key, region (e.g. us-east-1), output format (json)
```
⚠️ Never use your AWS root account keys for this. Create an IAM user with `AmazonS3FullAccess` (or a scoped S3 policy) and use its keys instead.

### Step 1 — Create the bucket
Bucket names must be globally unique across all of AWS.
```bash
aws s3 mb s3://your-unique-bucket-name --region us-east-1
```

### Step 2 — Enable static website hosting
```bash
aws s3 website s3://your-unique-bucket-name/ \
  --index-document index.html \
  --error-document 404.html
```

### Step 3 — Turn off "Block Public Access" for this bucket
```bash
aws s3api put-public-access-block \
  --bucket your-unique-bucket-name \
  --public-access-block-configuration \
  "BlockPublicAcls=false,IgnorePublicAcls=false,BlockPublicPolicy=false,RestrictPublicBuckets=false"
```

### Step 4 — Attach the bucket policy (public read)
First edit `bucket-policy.json` in this folder and replace `YOUR-BUCKET-NAME` with your actual bucket name. Then:
```bash
aws s3api put-bucket-policy \
  --bucket your-unique-bucket-name \
  --policy file://bucket-policy.json
```

### Step 5 — Upload the site
```bash
aws s3 sync . s3://your-unique-bucket-name/ \
  --exclude ".git/*" \
  --exclude "README.md" \
  --exclude "bucket-policy.json"
```

### Step 6 — Get your live URL
```bash
echo "http://your-unique-bucket-name.s3-website-us-east-1.amazonaws.com"
```
(Adjust the region in the URL if you used a different one — the exact format is `http://<bucket>.s3-website-<region>.amazonaws.com`, or `http://<bucket>.s3-website.<region>.amazonaws.com` for newer regions.)

Open that URL in your browser — your site should be live.

---

## 3. Option B — Azure Blob Storage Deployment

### Prerequisites
```bash
az login
```

### Step 1 — Create a resource group (if you don't have one)
```bash
az group create --name portfolio-rg --location eastus
```

### Step 2 — Create the storage account
Storage account names must be globally unique, lowercase, no spaces/hyphens.
```bash
az storage account create \
  --name yourstorageaccount \
  --resource-group portfolio-rg \
  --location eastus \
  --sku Standard_LRS \
  --kind StorageV2
```

### Step 3 — Enable static website hosting
This automatically creates the special `$web` container.
```bash
az storage blob service-properties update \
  --account-name yourstorageaccount \
  --static-website \
  --index-document index.html \
  --404-document 404.html
```

### Step 4 — Upload the site to the $web container
```bash
az storage blob upload-batch \
  --account-name yourstorageaccount \
  --source . \
  --destination '$web' \
  --pattern "*.html"
```

### Step 5 — Get your live URL
```bash
az storage account show \
  --name yourstorageaccount \
  --resource-group portfolio-rg \
  --query "primaryEndpoints.web" \
  --output tsv
```
It'll look like: `https://yourstorageaccount.z13.web.core.windows.net/`

---

## 4. Security notes (from the training kit — don't skip these)

- **Never use root/account keys directly.** Use IAM roles (AWS) or RBAC via Entra ID (Azure) for any deployment automation.
- The bucket policy's `"Principal": "*"` is intentional here — it's the one exception to "block public access," scoped only to `s3:GetObject` (read-only) on this one bucket. Don't widen it.
- Double-check the bucket/storage account only contains the static site files — never upload credentials, `.env` files, or personal documents into a public bucket.

---

## 5. Screenshots & Documentation to prepare before submission

Take these screenshots and add them to a `/screenshots` folder in your repo (or embed in this README):
1. Bucket / storage account created (AWS S3 console or Azure Portal)
2. Static website hosting enabled, showing index/error document config
3. Bucket policy / public access settings
4. Files uploaded (bucket contents view)
5. The live site loading in your browser, with the URL visible in the address bar

---

## 6. Pre-Submission Checklist

- [ ] Code is working properly (site loads, links work, 404 page works)
- [ ] Project files are complete (`index.html`, `404.html`, `bucket-policy.json`, `README.md`)
- [ ] GitHub repository created and set to **Public**
- [ ] README file added (this one — filled in with your real bucket/account name and live URL)
- [ ] Screenshots / documentation prepared (see section 5)
- [ ] Final project tested — open the live URL yourself in a browser before submitting
- [ ] Ready to submit without last-minute changes

## Live URL

_Add your final live URL here once deployed:_
```
https://usmanportfolio2026.z7.web.core.windows.net/
```