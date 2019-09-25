android_lint.skip_gradle_task = true
android_lint.filtering = true
Dir["*/build/reports/lint-results-debug.xml"].each do |file|
  android_lint.report_file = file
  android_lint.lint(inline_mode: true)
end


if /1dp|3dp|5dp|7dp|9dp/ === github.pr_diff
  message "dp should be multiple of 4"
end

active_files = (git.modified_files + git.added_files).uniq
layout_files = active_files.select { |file| file.include?("app/src/main/res/layout/") }

in_textview = false
maxline_exists  = false
textview_line = []
git.diff.patch.lines.each do |diff_line|
  if /^.*<TextView$/ === diff_line
    in_textview = true
    textview_line.clear
    message("In TextView, #{diff_line}")
  end
  if in_textview 
    textview_line.push(diff_line)
    if diff_line.include?("</TextView>") || diff_line.include?("/>")
      in_textview = false
      if !maxline_exists
        message("Please add android:maxLines\n```xml\n#{textview_line.join()}```")
      end
    elsif diff_line.include?("android:maxLines")
      maxline_exists  = true
    end
  end    
end

in_textview = false
maxline_exists  = false
textview_line_num = 0
layout_files.each do |filename|
  file = File.read(filename)
  lines = file.lines
  lines.each_with_index do |l, num|
    if l.include?("<TextView")
        in_textview = true
        textview_line_num = num + 1
    end
    if in_textview 
        if l.include?("</TextView>") || l.include?("/>")
            in_textview = false
            if !maxline_exists
                message("Please add android:maxLines", file: "#{filename}", line: textview_line_num)
            end
        elsif l.include?("android:maxLines")
            maxline_exists  = true
        end
    end    
  end
end

message "test"
warn "test"
