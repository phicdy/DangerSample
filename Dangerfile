if git.modified_files.include? "app/src/main/res/layout/*.xml"
  message "[ ] Please check TextView maxline"
end

if github.pr_diff.include? "<TextView"
  message "TextView!"
end

message "test"
warn "test"
