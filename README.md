# Test of copying file in sdk-style .csproj
This solution tests 3 ways of copying file to the output directory while building.

*Q: Why are you testing this?* <br>
A: Because I need a mechanism, that doesn't delete build copied files on build's cleanup.

*Q: Why do you need non-deleting files on builds' cleanup?* <br>
A: Because in company that I work, we have big solution of 160+ projects which has the same output dir
and when doing `rebuild` it often crashes, because `rebuild` incrementally cleans and builds projects 
one by one. Which is a problem, because when building in parallel, then it can rewrite or delete some
files.

So basically I'm looking for a solution that does not delete files on cleanup.
And also a solution that does it easily for multiple files.

1. Project `IncludedWithCopyToOutput` has a file reference and specifies to copy that reference file to 
the output. <br>
=> 
   * This deletes on cleanup
   * This is easy for multiple files, because we can use `*` wild-cards to specify multiple files
2. Project `TargetAfterBuildNative` uses post-build event to copy with msbuild's `Copy` task <br>
=> 
   * This does not delete on cleanup
   * I don't know how easy it is to copy multiple files yet.
3. Project `TargetAfterBuildXCopy` uses post-build event to copy with xcopy via msbuild's `Exec` task <br>
=> 
   * This does not delete on cleanup
   * This can easily be used for multiple lines, because xcopy can use `*` wild-cards
 
