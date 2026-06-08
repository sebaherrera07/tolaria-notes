---
type: AirOps
_organized: true
---
# Actual db schema

Add to root `actual_schema_script.sh`

```text
#!/usr/bin/env bash
# Run from the repo root (airops/)

HOOK_PATH=".git/hooks/post-checkout"

cat > "$HOOK_PATH" << 'EOF'
#!/usr/bin/env bash

# >>> BEGIN ACTUAL_DB_SCHEMA
# ActualDbSchema post-checkout hook (ROLLBACK)
# Runs db:rollback_branches on branch checkout.
# Modified to work from monorepo root — Rails app is at packages/rails_app.

# Check if this is a file checkout or creating a new branch
if [ "$3" == "0" ] || [ "$1" == "$2" ]; then
  exit 0
fi

RAILS_APP_DIR="$(dirname "$0")/../../../packages/rails_app"

if [ -f "$RAILS_APP_DIR/bin/rails" ]; then
  if [ -n "$ACTUAL_DB_SCHEMA_GIT_HOOKS_ENABLED" ]; then
    GIT_HOOKS_ENABLED="$ACTUAL_DB_SCHEMA_GIT_HOOKS_ENABLED"
  else
    GIT_HOOKS_ENABLED=$(cd "$RAILS_APP_DIR" && ./bin/rails runner "puts ActualDbSchema.config[:git_hooks_enabled]" 2>/dev/null)
  fi

  if [ "$GIT_HOOKS_ENABLED" == "true" ]; then
    (cd "$RAILS_APP_DIR" && ./bin/rails db:rollback_branches)
  fi
fi
# <<< END ACTUAL_DB_SCHEMA
EOF

chmod +x "$HOOK_PATH"
echo "post-checkout hook installed at $HOOK_PATH"
```

Run with `bash actual_schema_script.sh`
