---
layout:     post
title:      first
category: blog
description: 。。。
---
在viewDidLoad中加上如下代码：
 <pre><code>if ([self.tableView respondsToSelector:@selector(setSeparatorInset:)]) {
        
        [self.tableView setSeparatorInset:UIEdgeInsetsZero];
        
    }
    
    if ([self.tableView respondsToSelector:@selector(setLayoutMargins:)]) {
        
        [self.tableView setLayoutMargins:UIEdgeInsetsZero];
        
    }
</pre></code>
然后在UITableView代理方法中加入：
 <pre><code>- (void)tableView:(UITableView *)tableView willDisplayCell:(UITableViewCell *)cell forRowAtIndexPath:(NSIndexPath *)indexPath

{
    
      if ([cell respondsToSelector:@selector(setSeparatorInset:)]) {
        
          [cell setSeparatorInset:UIEdgeInsetsZero];
        
      }
    
      if ([cell respondsToSelector:@selector(setLayoutMargins:)]) {
        
          [cell setLayoutMargins:UIEdgeInsetsZero];
        
      }
    
}
</pre></code>