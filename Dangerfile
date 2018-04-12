# Make it more obvious that a PR is a work in progress and shouldn't be merged yet.
has_wip_label = github.pr_labels.any? { |label| label.include? "WIP" }
has_wip_title = github.pr_title.include? "[WIP]"

if has_wip_label || has_wip_title
	warn("PR is classed as Work in Progress")
end

# Warn when there is a big PR.
warn("Big PR") if git.lines_of_code > 500

# Mainly to encourage writing up some reasoning about the PR, rather than just leaving a title.
if github.pr_body.length < 3 && git.lines_of_code > 10
  warn("Please provide a summary in the Pull Request description")
end

src_root = File.expand_path('../PRODUCTNAME/app', __FILE__)

xcov.report(
  workspace: "#{src_root}/PRODUCTNAME.xcworkspace",
  scheme: "debug-PRODUCTNAME",
  output_directory: "#{ENV['RZ_TEST_REPORTS']}/xcov",
   # For some reason coverage is on the "develop-" app target instead of "debug-"
  include_targets: "develop-PRODUCTNAME.app, Services.framework",
  ignore_file_path: "#{src_root}/fastlane/.xcovignore",
  coveralls_service_name: "circleci",
  coveralls_service_job_id: "#{ENV['CIRCLE_BUILD_NUM']}"
)

## ** SWIFT LINT ***
# Use the SwiftLint included via CocoaPods
swiftlint.binary_path = "#{src_root}/Pods/SwiftLint/swiftlint"
swiftlint.config_file = "#{src_root}/.swiftlint.yml"

# Run Swift-Lint and warn us if anything fails it
swiftlint.directory = src_root
swiftlint.lint_files inline_mode: true