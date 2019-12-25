欢迎来到我的github，感谢您的到来～～～
[zy@ZY git]$ git init
Initialized empty Git repository in /home/zy/git/.git/
[zy@ZY git]$ vi readme.txt
[zy@ZY git]$ git status 
# On branch master
#
# Initial commit
#
# Untracked files:
#   (use "git add <file>..." to include in what will be committed)
#
#	readme.txt
nothing added to commit but untracked files present (use "git add" to track)
[zy@ZY git]$ git add readme.txt
[zy@ZY git]$ git status 
# On branch master
#
# Initial commit
#
# Changes to be committed:
#   (use "git rm --cached <file>..." to unstage)
#
#	new file:   readme.txt
#
[zy@ZY git]$ git config --global user.name "JonyTC"
[zy@ZY git]$ git config --global user.email "1192687103@qq.com"
[zy@ZY git]$ git commit -m "add readme.txt"
[master (root-commit) c246e5a] add readme.txt
 1 file changed, 1 insertion(+)
 create mode 100644 readme.txt
[zy@ZY git]$ git log
commit c246e5a16fab3441056255e82691d0990224f2b4
Author: JonyTC <1192687103@qq.com>
Date:   Wed Dec 25 22:31:02 2019 +0800

    add readme.txt
[zy@ZY git]$ git push origin master
fatal: 'origin' does not appear to be a git repository
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.
[zy@ZY git]$ git branch 
* master
[zy@ZY git]$ git remote add origin https://github.com/JonyTC/project.git
[zy@ZY git]$ git push origin master 
fatal: unable to access 'https://github.com/JonyTC/project.git/': Failed connect to github.com:443; Connection timed out
[zy@ZY git]$ git remote add origin git@github.com:JonyTC/project.git
fatal: remote origin already exists.
[zy@ZY git]$ git remote re
remove   rename   
[zy@ZY git]$ git remote 
add            remove         set-branches   set-url        update 
prune          rename         set-head       show           
[zy@ZY git]$ git remote update origin git@github.com:JonyTC/project.git
fatal: No such remote or remote group: git@github.com:JonyTC/project.git
[zy@ZY git]$ git remote rename origin git@github.com:JonyTC/project.git
fatal: 'git@github.com:JonyTC/project.git' is not a valid remote name
[zy@ZY git]$ git remote -v
origin	https://github.com/JonyTC/project.git (fetch)
origin	https://github.com/JonyTC/project.git (push)
[zy@ZY git]$ git remote remove origin 
[zy@ZY git]$ git remote add origin git@github.com:JonyTC/project.git
[zy@ZY git]$ git push origin master 
The authenticity of host 'github.com (13.250.177.223)' can't be established.
RSA key fingerprint is SHA256:nThbg6kXUpJWGl7E1IGOCspRomTxdCARLviKw6E5SY8.
RSA key fingerprint is MD5:16:27:ac:a5:76:28:2d:36:63:1b:56:4d:eb:df:a6:48.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'github.com,13.250.177.223' (RSA) to the list of known hosts.
Permission denied (publickey).
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.
[zy@ZY git]$ git remote remove origin 
[zy@ZY git]$ git remote add origin https://github.com/JonyTC/project.git
[zy@ZY git]$ git push origin master 
Username for 'https://github.com': JonyTC
Password for 'https://JonyTC@github.com': 
To https://github.com/JonyTC/project.git
 ! [rejected]        master -> master (fetch first)
error: failed to push some refs to 'https://github.com/JonyTC/project.git'
hint: Updates were rejected because the remote contains work that you do
hint: not have locally. This is usually caused by another repository pushing
hint: to the same ref. You may want to first merge the remote changes (e.g.,
hint: 'git pull') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
[zy@ZY git]$ git pull
warning: no common commits
remote: Enumerating objects: 72, done.
remote: Counting objects: 100% (72/72), done.
remote: Compressing objects: 100% (43/43), done.
remote: Total 72 (delta 20), reused 64 (delta 12), pack-reused 0
Unpacking objects: 100% (72/72), done.
From https://github.com/JonyTC/project
 * [new branch]      demo       -> origin/demo
 * [new branch]      master     -> origin/master
From https://github.com/JonyTC/project
 * [new tag]         v1.0       -> v1.0
 * [new tag]         v1.2       -> v1.2
 * [new tag]         v1.3       -> v1.3
There is no tracking information for the current branch.
Please specify which branch you want to merge with.
See git-pull(1) for details

    git pull <remote> <branch>

If you wish to set tracking information for this branch you can do so with:

    git branch --set-upstream-to=origin/<branch> master

