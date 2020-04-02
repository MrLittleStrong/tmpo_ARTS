### Link

[客户端单周发版下的多分支自动化管理与实践](https://tech.meituan.com/2019/01/10/traffic-git-branch-management.html)



### Notes

This article introduced how to solve the problems of releasing once a week. 

The problems mainly are listed below:

1. Developing lifecycle is not regular,  there will be a lot of builds. This may cause the Service-Developers' branches chaos.
2. Normally, Service rely on several base repositories.  This may cost a lot in  merging.
3. Public base repository also need this.

Solution:

Overall, the solution is based on Jenkins Job:

1. When released a build(like 1.0.0), Jenkins Job automatically create one branch (like1.0.2). The name of branch is also given automatically.
2. Once released a build in Stage branch, Jenkins Job will automatically send notice to developers to merge from stage. 
3. When developing branch wants to packing, Jenkins Job will detect the Stage branch situation, If branch is not updated, packing will fail.
4. Merging branches also depends on Jenkins Job like above.
5. Public base repository also use the same strategy. However, if public base repository released, It will force Service base update.
6. Hotfix still remain manual for its complexity.

### Summary

This  article introduced a good usage of Jenkins. I know little about Jenkins before. We only use it to pack ipa or apk and then submit it to QA team.  To keep efficient, we need to use the automated tools wisely. 