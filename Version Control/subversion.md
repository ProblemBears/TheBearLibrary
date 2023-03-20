<h2 align="center" id="merging"> Merging </h2>

- Make sure you update your main `dev branch`
- Then `switch` to your `experimental branch` so that you could `merge` in the new changes from your `dev branch` -
    * `RMB` Local Repo Directory &rarr; `SVN` &rarr; `Merge` &rarr; `Merge a range of revisions` :
        * `URL to merge from` : The main `dev branch` (since we are currently on the experimental branch)
        * `Revision Range to merge [specific range]` : (leave this area blank SVN will automatically merge any revisions that have not been merged previously)
        * Then finally, hit `Next` and `Merge`
- Once the merge happens from your `main dev branch` to your `experimental branch`, SVN will show red icons to reflect these "merged changes", to actually record these changes do a `commit`