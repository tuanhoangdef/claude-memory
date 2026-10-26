# claude-memory

Bản sao các memory file của Claude Code (`~/.claude/projects/<project>/memory/`), dùng để chuyển sang máy khác.

## Cấu trúc

```
projects/
  -Users-bssgroup-Personal-de/memory/                 ~/Personal/de (học data engineering)
  -Users-bssgroup-Personal-machine-learning/memory/   ~/Personal/machine-learning
  -Users-bssgroup-Projects-cro/memory/                ~/Projects/cro
  -Users-bssgroup-Projects-project-beta/memory/       ~/Projects/project-beta
```

Tên thư mục project = đường dẫn tuyệt đối của project, thay `/` bằng `-`.

## Khôi phục trên máy mới

```zsh
git clone <repo-url> ~/claude-memory
mkdir -p ~/.claude/projects
cp -R ~/claude-memory/projects/* ~/.claude/projects/
```

Nếu máy mới **khác username hoặc khác đường dẫn project** (vd `/Users/tuan/Personal/de`): mở Claude Code ở project đó một lần để nó tạo thư mục `~/.claude/projects/<tên mới>/`, rồi chép nội dung `memory/` tương ứng vào đó.
