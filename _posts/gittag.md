## git-tag脚本

```flow
#!/bin/bash

branch="test"
if `git status | grep "RELEASE" &>/dev/null`; then
   branch="pro"
fi

user=$(echo $(git config user.name))
date=$(echo $(date +'%Y%m%d'))

prefix=$(echo ${branch}-${user}-${date})

count=$(echo $(git tag -l "${prefix}-*" | wc -l | xargs printf '%02d'))

function mi-tag() {
    git push
    git pull --tags
    local new_tag=$(echo ${prefix}-${count})
    echo ${new_tag}
    git tag ${new_tag}
    git push origin $new_tag
}

mi-tag;
```