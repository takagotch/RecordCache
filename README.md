### RecordCache
---
https://github.com/orslumen/record-cache

```
gem 'record-cache'

bundle
appraisal
appraisal rake
appraisal rails-32 rake
appraisal rails-40 rspec ./spec/lib/strategy/base_spec.rb:61

git tag -a v0.1.1 -m 'version 0.1.1'
git push origin master --tags
gem update --system
gem build record-cache.gemspec
gem push record-cache-0.1.1.gem

```


```ruby
RecordCache::Base.version_store = Rails.cache
RecordCache::Base.register_store(:local, ActiveSupport::Cache.lookup_store(:memory_store))
RecordCache::Base.register_store(:shared, Rails.cache)

class Person < ActiveRecord::Base
  cache_records :store => :shared, :key => "pers"
end

class Permission < ActiveRecord
  cache_records :store => :shared, :key => "perm", :index => [:person_id]
  belongs_to :person
end

class Priority < ActiveRecord::Base
  cache_records :store => :local, :key => "prio", :full_table => true
end

RecordCache::Base.version_store.on_write_failure{ |key| clear_this_key_after_2_seconds(key) }
RecordCache::Base.disable!

require 'record_cache/test/resettable_version_store'
RSpec.configure do |config|
  config.after(:each) do
    RecordCache::Base.version_store.reset!
  end
end

require 'record_cache/test/resettable_version_store'
After do |scenario|
  RecordCahe::Base.version_store.reset!
end

```

```
```
