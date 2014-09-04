Best Practices
==============
Written by: Sebastian Radloff

Git Practices
-------------

* Always create a new branch for a feature
	* `git checkout -b branch_name`
* Only commit files that you are actually changing code in
	* If you commit code with temp files and/or use the add all (git add -A), let's just say I have a very special set of skills... Just don't do it. Add files individually using `git add file_name`
* Commit often!
	* This is for all our benefit.
	* Everytime you write a new function, view, or any controller logic, please commit the file and write a detailed commit message.
	* `git commit -m "unique commit message"`
* Only push up to your branch.
	* `git push -u origin your_current_working_branch`
* Peer review every merge request. DO NOT MERGE TO MASTER without a peer review and passing build tests from Codeship.
* Do not merge anyting that isn't tested!
* Everytime the master is updated:
	* On your branch:
		* `git status`: to see what files you've modified, added, deleted.
		* If you are in a place to commit:
			*	`git add file_name` to add each file individually.
			* `git commit -m "unique commit message"` to add a commit message.
			* DO NOT PUSH YOUR BRANCH! This will mess with the ability to rebase.
		* If you are NOT in a place to commit, stash the files using `git stash`.
		* `git pull --rebase origin master`	[link](http://git-scm.com/book/mk/%D0%93%D1%80%D0%B0%D0%BD%D0%B5%D1%9A%D0%B5-%D1%81%D0%BE-Git-Rebasing)
			* Rebasing is important so that your commit histroy looks linear even though you had branches in parallel.
		* If you used `git stash` earlier:
			* `git stash apply` will bring those files you stashed back out into your unstaged area.
			* `git status` to make sure that those files you stashed are back as unstaged.
		* Get back to work!
		* If you get merge conflicts: [link](http://css-tricks.com/deal-merge-conflicts-git/)
			* You'll see a list of the "Unmerged paths" which are the files in which you have merge conflicts.
			* Close your current sublime doc and open it up again using `subl .` if you have the command enabled. Just open up the files.
			* You'll see something like: 
				* `<<<<<<<<<<<<<<<<<<<<HEAD`
				* `This line is on your current branch`
				* `=================================`
				* `This line is from the master`
				* `>>>>>>>>>>>>>>>>>>>>>(refs/head/master)`
			* Look over with the person who wrote that code on the master to determine what code is important.
				* Look at master on github and compare side-by-side to see why you're changes are different
			* When your confident, delete what you deemed is uneccessary, make sure to remove the HEAD and the arrows from master, or they will appear
			* `git status`
			* Now `git add file_name` all files that you changed individually.
			* Commit them with the message "merged new changes with master"
			* `git rebase --continue` to continue working on your feature!
	* You may have more merge conflicts while rebasing since it goes through every commit on your branch as it's rebasing, but that shouldn't be much of an issue if you are properly dividing features.