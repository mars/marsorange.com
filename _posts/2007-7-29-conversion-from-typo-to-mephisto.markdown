---
layout: post
title: !binary |-
  Q29udmVyc2lvbiBmcm9tIFR5cG8gdG8gTWVwaGlzdG8=
---
First patch, to avoid stupid migration failure:
<filter:code lang="diff">Index: db/migrate/060_move_theme_path.rb
===================================================================
--- db/migrate/060_move_theme_path.rb   (revision 2936)
+++ db/migrate/060_move_theme_path.rb   (working copy)
@@ -23,5 +23,6 @@
       end
       FileUtils.rmdir new_path
     end
+  rescue
   end
 end
</filter:code>

Then, copy Typo's `categorizations` table to a new table named `articles_categories`. <del>(See what happens when you ignore Rails' database conventions?!)</del>

Next, create the file `vendor/plugins/mephisto_converters/lib/converters/typo/feedback.rb`:
<filter:code lang="ruby">module Typo
  class Feedback < ActiveRecord::Base
    establish_connection configurations['typo']
    set_table_name 'feedback'
    belongs_to :text_filter, :class_name => 'Typo::TextFilter'
    def filter
      self.text_filter ? self.text_filter.name : nil
    end
  end
end
</filter:code>

Two patches to switch over to use the new `Feedback` class:
<filter:code lang="diff">Index: vendor/plugins/mephisto_converters/lib/converters/typo/comment.rb
===================================================================
--- vendor/plugins/mephisto_converters/lib/converters/typo/comment.rb   (revision 2936)
+++ vendor/plugins/mephisto_converters/lib/converters/typo/comment.rb   (working copy)
@@ -1,5 +1,5 @@
 module Typo
-  class Comment < Content
+  class Comment < Feedback
 #    belongs_to :article, :foreign_key => 'parentid', :class_name => 'Typo::Comment'
   end
 end
</filter:code>

<filter:code lang="diff">Index: vendor/plugins/mephisto_converters/lib/converters/typo.rb
===================================================================
--- vendor/plugins/mephisto_converters/lib/converters/typo.rb   (revision 2936)
+++ vendor/plugins/mephisto_converters/lib/converters/typo.rb   (working copy)
@@ -1,6 +1,7 @@
 require 'converters/typo/content'
 require 'converters/typo/page'
 require 'converters/typo/article'
+require 'converters/typo/feedback'
 require 'converters/typo/comment'
 require 'converters/typo/category'
 require 'converters/typo/user'
@@ -107,4 +108,4 @@
     sec
   end
 
-end
\ No newline at end of file
+end
</filter:code>

Finally, start her up:
<filter:code lang="shell">%  rake db:bootstrap RAILS_ENV=production
%  script/runner --trace "Mephisto.convert_from :typo" -e production
</filter:code>
