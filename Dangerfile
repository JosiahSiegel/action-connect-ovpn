if github.pr_labels.include?("status: Done") or github.pr_labels.include?("status: Wait For Review")
  auto_label.remove("status: WIP")  
end

if github.pr_labels.include?("status: Wait For Review")
  message "This PR ready for reviews."
end

if github.pr_labels.empty?
  pr_number = github.pr_json["number"]
  auto_label.set(pr_number, "status: WIP", "ff8800") 
  warn("PR is classed as Work in Progress")
end  

warn "#{github.html_link("Package.json")} was edited." if git.modified_files.include? "Package.json"

# Ignore inline messages which lay outside a diff's range of PR
github.dismiss_out_of_range_messages

# Warn when there is a big PR
warn("Big PR") if git.lines_of_code > 300 and git.lines_of_code < 500 and git.lines_of_code < 700 and git.lines_of_code < 1000
warn("Large PR") if git.lines_of_code > 500 and git.lines_of_code < 700 and git.lines_of_code < 1000
warn("Huge PR") if git.lines_of_code > 700 and git.lines_of_code < 1000
warn("Freakin Huge PR") if git.lines_of_code > 1000
markdown ("Pull Request size seems relatively large. If Pull Request contains multiple changes, split each into separate PR will helps faster, easier review.") if git.lines_of_code > 300

# failure "Please provide a summary in the Pull Request description" if github.pr_body.length < 5

eslint.config_file = ".eslintrc"
eslint.ignore_file = ".eslintignore"
eslint.lint

def get_prettier_files(files)
  files.select do |file|
    file.end_with?('.js')
  end
end

prettier_candidates = get_prettier_files(git.added_files + git.modified_files)

return if prettier_candidates.empty?

unpretty = `node_modules/prettier/bin-prettier.js --list-different #{prettier_candidates.join(" ")}`
             .split(/$/)
             .map(&:strip)
             .reject(&:empty?)

return if unpretty.empty?

warn 'This merge request changed frontend files without pretty printing them.'

markdown(<<~MARKDOWN)
  ## Pretty print Frontend files
  The following files should have been pretty printed with `prettier`:
  * #{unpretty.map { |path| "`#{path}`" }.join("\n* ")}
  Please run
  ```
  node_modules/.bin/prettier --write \\
  #{unpretty.map { |path| "  '#{path}'" }.join(" \\\n")}
  ```
  Also consider auto-formatting [on-save].
  [on-save]: https://docs.gitlab.com/ee/development/new_fe_guide/style/prettier.html
MARKDOWN
