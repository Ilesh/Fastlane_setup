# This file contains the fastlane.tools configuration
# You can find the documentation at https://docs.fastlane.tools
#
# For a list of all available actions, check out
#
#     https://docs.fastlane.tools/actions
#
# For a list of all available plugins, check out
#
#     https://docs.fastlane.tools/plugins/available-plugins
#

# Uncomment the line if you want fastlane to automatically update itself
# update_fastlane

default_platform(:ios)

platform :ios do
   desc "Run all commans"
   lane :beta do
       begin
           iprunscript
       rescue => exception
           on_error(exception)
       end
   end
end


### Methods
def iprunscript  
   clear_derived_data
   increment_build_number
   build_app 
   changelog_from_git_commits # this will generate the changelog based on your last commits
   upload_to_testflight(changelog: "Add rocket emoji")  #varient 1
   #upload_to_testflight(skip_waiting_for_build_processing:true) #varient 2
   on_success
end

### Alert
def on_success  
   #str_slack_url = "xxxxxx"
   slack(
   message: "App successfully uploaded to iTunesConnect.",
   success: true,
   slack_url: "xxxxxxxxx",
   default_payloads: [:git_branch, :last_git_commit_message],
   attachment_properties: {
   thumb_url: "https://i.dailymail.co.uk/i/pix/2015/09/01/18/2BE1E88B00000578-3218613-image-m-5_1441127035222.jpg",
       fields: [
           {
               title: "Build number",
               value: ENV["BUILD_NUMBER"],
           }        
       ]
   }
)

end


def on_error(exception)
   slack(
       message: "Something went wrong.",
       success: false,
       slack_url: "xxxxxxxx",
       default_payloads: [:git_branch, :last_git_commit_message],
       attachment_properties: {
	     thumb_url: "https://i.dailymail.co.uk/i/pix/2015/09/01/18/2BE1E88B00000578-3218613-image-m-5_1441127035222.jpg",
           fields: [
               {
                   title: "Build number",
                   value: ENV["BUILD_NUMBER"],
               },
               {
                   title: "Error message",
                   value: exception.to_s,
                   short: false
               }
           ]
       }
   )
end