if /1dp|3dp|5dp|7dp|9dp/ === github.pr_diff
  message "dp should be multiple of 4"
end

active_files = (git.modified_files + git.added_files).uniq
layout_files = active_files.select { |file| file.include?("app/src/main/res/layout/") }

in_textview = false
maxline_exists  = false
textview_lines = []
layout_files.each do |filename|
  file = File.read(filename)
  lines = file.lines
  lines.each do |l|
    if l.include?("<TextView")
        in_textview = true
        textview_lines.clear
    end
    if in_textview 
        textview_lines.push(l)
        if l.include?("</TextView>") || l.include?("/>")
            in_textview = false
            if !maxline_exists
                warn("Please add android:maxLines, #{filename}\n```xml\n#{textview_lines.join('\n')}```")
            end
        elsif l.include?("android:maxLines")
            maxline_exists  = true
        end
    end    
  end
end

message "test"
warn "test"
