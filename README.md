# Instructions

## New Repository

```sh
echo "# configs" >> README.md
git init
git add .
git commit -m "first commit"
git branch -M main
git remote add origin git@github.com:hadipic/configs.git
git push -u origin main
```

## Push Existing Repository

```sh
git remote add origin git@github.com:hadipic/configs.git
git branch -M main
git push -u origin main
```

## Push Changes
```sh
git add .
git commit -m "Message"
git push origin BRANCH_NAME
```

## Change Branches
```sh
git checkout -b BRANCH origin/BRANCH # تغییر برنچ و متصل کردن آن به گیت ریموت
## 
git checkout BRANCH # تغییر برنچ
##
git checkout -b BRANCH # ساخت برنچ جدید روی گیت لوکال
```

## Fetch New Changes
```sh
git fetch --prune # دریافت اخرین تغییرات و حذف برنچ های اضافی
##
git fetch --all # دریافت اخرین تغییرات
```

## Create SSH Key

```sh
ssh-keygen -t ed25519 -C "your_email@example.com"
```

[!] Press Enter for all

## SSH Public Key path
```sh
~/.ssh/id_ed25519.pub
```

## Configuring GIT
```sh
git config --global user.name "Hadi Bashniji"
git config --global user.email "Hadi@Bashniji.Dev"
```

