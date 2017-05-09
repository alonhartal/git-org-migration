# git-org-migration
a script for copy all git repositories (private &amp; public) including all branches, tags and commits history from one organization to another

curl -u <USERNAME>:<PERSONAL_ACCESS_TOKEN> -s https://api.github.com/orgs/<SOURCE_REPOSITORY_NAME>/repos\?per_page\=200 | ruby -rubygems -e "require 'json';
JSON.load(STDIN.read).each do |repository|
  clone_url = repository['clone_url']
  repository_name = repository['name']
  %x[git clone --mirror #{clone_url}]
  %x[curl -u <USERNAME>:<PERSONAL_ACCESS_TOKEN> https://api.github.com/orgs/<TARGET_REPOSITORY_NAME>/repos -d '{\"name\": \"#{repository_name}\", \"private\": true}']
  %x[cd <PATH_TO_CURRENT_LOCAL_FOLDER>/#{repository_name}.git && git remote set-url --push origin https://github.com/<TARGET_REPOSITORY_NAME>/#{repository_name}]
  %x[cd <PATH_TO_CURRENT_LOCAL_FOLDER>/#{repository_name}.git && git push --mirror && cd ..]
end"
