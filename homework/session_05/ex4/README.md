# Bài 4 — Hotfix & Gitflow

## Bối cảnh

- `main` = production ổn định, tag **v1.0.0**
- `develop` = đang code tính năng mới (chưa deploy)
- Lỗi lộ dữ liệu trên production → vá nóng bằng **hotfix/v1.0.1**

## Sơ đồ Gitflow (ASCII)

```text
main      ──●──────────────●────●──── tag v1.0.1
            │ v1.0.0       │    ▲
            │              │    │ merge
            │         hotfix/v1.0.1
            │              │
develop   ──●──────●───────●────●──── (nhận hotfix)
                   feature…    ▲
                               │ merge hotfix
```

1. Tách `hotfix/v1.0.1` **từ main** (không từ develop)
2. Sửa lỗi → merge vào **main** → **tag v1.0.1**
3. Merge cùng hotfix vào **develop** (tránh trôi lỗi)
4. Xóa nhánh hotfix cục bộ

## Lệnh đã chạy

```bash
# Giả sử đã có main @ v1.0.0 và develop
git checkout main
git checkout -b hotfix/v1.0.1

# sửa lỗi (ví dụ)
echo "fix: mask user data" >> fix-security.txt
git add fix-security.txt
git commit -m "fix: prevent user data exposure"

# Release hotfix lên production
git checkout main
git merge hotfix/v1.0.1
git tag -a v1.0.1 -m "Release Hotfix 1.0.1"

# Đồng bộ sang develop
git checkout develop
git merge hotfix/v1.0.1

# Dọn nhánh
git branch -d hotfix/v1.0.1
```

## Kiểm tra

```bash
git branch -a
git tag
git log --graph --oneline --all
```

Kỳ vọng:

- Tag **v1.0.1** trỏ commit trên `main`
- `develop` có commit/sửa lỗi từ hotfix
- Không còn nhánh `hotfix/v1.0.1` (đã xóa local)

## Ảnh / log minh chứng (dán khi nộp)

```text
# git tag
v1.0.0
v1.0.1

# git log --graph --oneline --all
# (dán output sau khi chạy thật)
```
