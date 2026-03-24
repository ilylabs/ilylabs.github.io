source "https://rubygems.org"
gem "github-pages", group: :jekyll_plugins

gem "tzinfo-data"
gem "wdm", "~> 0.1.0" if Gem.win_platform?

# Security overrides: pin known-safe minimum versions for packages with CVEs.
# These versions meet or exceed the patched releases for all known advisories
# as of 2026-03-24 (CVE-2026-33210, CVE-2025-58767, CVE-2024-47220, etc.).
gem "json", ">= 2.19.2"
gem "rexml", ">= 3.4.4"
gem "webrick", ">= 1.9.2"
gem "nokogiri", ">= 1.19.2"

# If you have any plugins, put them here!
group :jekyll_plugins do
  gem "jekyll-paginate"
  gem "jekyll-sitemap"
  gem "jekyll-gist"
  gem "jekyll-feed"
  gem "jemoji"
  gem "jekyll-include-cache"
  gem "jekyll-remote-theme"
end