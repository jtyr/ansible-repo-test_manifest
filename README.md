repo-test_manifest
==================

Here is the recipe how to bootstrap the Ansible enviroments:

```
# Get the repo script
mkdir ~/bin
curl -o ~/bin/repo http://commondatastorage.googleapis.com/git-repo-downloads/repo
chmod +x ~/bin/repo
echo 'export PATH="$PATH:$HOME/bin"' >> ~/.bashrc

# Build environments
for ENV in master dev qa stg prd; do
  mkdir -p ~/ansible/env/$ENV
  cd ~/ansible/env/$ENV
  repo init -u https://github.com/jtyr/ansible-repo-test_manifest.git -b $ENV
  repo sync --no-clone-bundle
done

# Switch from the detach state to the correct branch (optional for master env only)
repo forall -p -c 'git checkout $(git branch -avv | grep "remotes/m/master" | sed "s,.*/,,")'

# Set default push behavior (optional)
git config --global push.default current
```

More information about `repo` script:

- [Manifest format](https://gerrit.googlesource.com/git-repo/+/master/docs/manifest-format.txt)
- [Repo command reference](http://source.android.com/source/using-repo.html)
- [How to use repo script with AOSP](http://udinic.wordpress.com/2014/05/24/aosp-part-1-get-the-code-using-the-manifest-and-repo)
- [Repo tips and tricks](http://xda-university.com/as-a-developer/repo-tips-tricks)
