# GitHub publication workflow

Published repositories:

- API: [`kalaabalb/yomart-api`](https://github.com/kalaabalb/yomart-api), `main`
- Admin: [`kalaabalb/yomart-admin`](https://github.com/kalaabalb/yomart-admin), `main`
- Customer web: [`kalaabalb/yomart-web`](https://github.com/kalaabalb/yomart-web), `main`
- Flutter client: [`kalaabalb/yomoblies`](https://github.com/kalaabalb/yomoblies), `main`
- Documentation: [`kalaabalb/yomart-docs`](https://github.com/kalaabalb/yomart-docs), `main`

For each app, publish from an isolated copy whose remote and branch were checked first. Inspect status, staged file names, ignored files and exact diff before committing. Run that repository's tests/build, `git diff --check`, and a secret scan against both worktree and staged content. Exclude `.env*` except safe examples, signing materials, `node_modules`, generated build directories, private receipts/uploads, local databases, user/customer exports, and operational credentials. Do not import contaminated legacy Git history. Never force-push; compare remote `main` before publication and stop on unexpected history.

Do not publish private audit backups, cleanup manifests, database dumps, recovery manifests, synthetic images/screenshots without privacy review, or local-path incident logs. Revoke exposed credentials before considering history rewrite; rewriting history does not revoke a secret and requires clone/fork coordination.
