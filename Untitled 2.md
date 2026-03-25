# Yaonan Cai Learnings  
  
---  
## Checkpoint 1:  
**Fixing test errors**:  
**Commands used:**  
 1. git add tests/test_calculator.py  
 2. git commit -m "fix"  
 3. git add src/calculator.py  
 4. git commit -m "update"  
 5. git push origin feature/119823026  
  
**Learnings:**  
While I know the command line for commit and push, I usually just use the   
PyCharm interface to commit and push. This task helped me to refresh how to  
use them.  
  
## Checkpoint 2:  
**Commands used:**  
 1. git log --oneline  
 2. git revert 66f6716  
 3. git push origin feature/119823026  
   
**Learnings:**  
Learned how to use revert to undo changes while keep the original commit.   
If I didn't learn this command, I'll probably just manually delete and commit  
the code. But this command prevent mistakes like accidentally deleting extra codes  
when deleted the bad code.