---
title: "Rails best practices học hỏi được và đúc kết sau những comment của reviewer"
categories:
  - rails
tags:
  - rails
  - ruby
  - best-practices
---
Dưới đây là những best practices mình học hỏi được từ blog của những senior rails dev, từ những comment lúc mình code. Những kinh nghiệm này rất tốt để học hỏi, để có những dòng code dễ đọc, dễ bảo trì và mở rộng hơn.

## Đừng thay đổi params hash
Bad code:
```
def search
  params.except!(:action, :controller)
  @search = User.search(params)
  render "search"
end
```
Code bên trên đã tạo ra side effect là thay đổi mất params ban đâu, việc này theo cuốn CLEAN CODE thì là điều cấm kị. Theo sách này thì chúng ta nên tạo một bản sao của biến cũ và thay đổi trên đó, tránh tạo side effect sẽ giúp dễ debug hơn.

Good code:
```
def search
  @search = User.search(search_params)
  render "search"
end

private

def search_params
  # params.except(:action, :controller)
  params.permit(:user_id, :name)
end
```

## Không rescue Exception, hãy rescue StandardError
Bad code:
```
begin
  foo
rescue Exception => e
  logger.warn "Unable to foo, will ignore: #{e}"
end
```
Khi rescue như trên bạn vô tình rescue cả những lỗi như: SyntaxError, LoadError và Interrupt của hệ thống khiến Ctrl-C ngừng rails server không được. Chỉ cần dùng `rescue` như bên dưới là chúng ta có thể rescue từ StandardError.

Good code:
```
begin
  foo
rescue => e
  logger.warn "Unable to foo, will ignore: #{e}"
end
```

## Thin Controller Fat Model
Bad code:
```
class PostsController < ApplicationController
  def publish
    @post = Post.find(params[:id])
    @post.update_attribute(:is_published, true)
    @post.approved_by = current_user
    if @post.created_at > Time.now - 7.days
      @post.popular = 100
    else
      @post.popular = 0
    end

    redirect_to post_url(@post)
  end
end
```
Ví dụ trên cho thấy rất nhiều công việc đáng lý ra phải xử lý trong Model lại bị mang ra Controller. Controller như tên gọi chỉ nên tiếp nhận request và điều hướng trả về response thôi.
Đây là điều mà ai code Rails đều biết, dù nó cũng có nhược điểm là càng ngày Model của chúng ta càng phình to quá mức. Do vậy đây chưa phải phương án tốt nhất, phương án tốt hơn là "Skinny Controllers, Skinny Models" được viết ở đây: [Thoughtbot](https://thoughtbot.com/blog/skinny-controllers-skinny-models)

Good code:
```
class Post < ActiveRecord::Base
  def publish
    self.is_published = true
    self.approved_by = current_user
    if self.created_at > Time.now - 7.days
      self.popular = 100
    else
      self.popular = 0
    end
  end
end

class PostsController < ApplicationController
  def publish
    @post = Post.find(params[:id])
    @post.publish

    redirect_to post_url(@post)
  end
end
```

## Dùng Model Callback
Bad Code:
```
<% form_for @post do |f| %>
  <%= f.text_field :content %>
  <%= check_box_tag 'auto_tagging' %>
<% end %>

class PostsController < ApplicationController
  def create
    @post = Post.new(params[:post])

    if params[:auto_tagging] == '1'
      @post.tags = AsiaSearch.generate_tags(@post.content)
    else
      @post.tags = ""
    end

    @post.save
  end
end
```
Ví dụ này thực chất cũng là Fat Controller nên việc chúng ta là dùng Model Callback để mang logic đó vào Model. Đó là ActiveRecord Callback, có thể tìm hiểu thêm [ở đây](https://api.rubyonrails.org/classes/ActiveRecord/Callbacks.html)

Good Code:
```
class Post < ActiveRecord::Base
  attr_accessor :auto_tagging
  before_save :generate_tagging

  private
  def generate_taggings
    return unless auto_tagging == '1'
    self.tags = Asia.search(self.content)
  end
end

<% form_for @post do |f| %>
  <%= f.text_field :content %>
  <%= f.check_box :auto_tagging %>
<% end %>

class PostsController < ApplicationController
  def create
    @post = Post.new(params[:post])
    @post.save
  end
end
```

## Không dùng default scope
Bad code:
```
class Post
  default_scope where(published: true).order("created_at desc")
end
```
* Không thể overide default scope
* Khi dùng default scope thì tất cả truy vấn của Class đó sẽ tự động thêm điều kiện đó vào dẫn đến khó kiếm soát
* Ảnh hướng đến việc khởi tạo object. Như ở ví dụ badcode trên với `post = Post.new` thì `post.published` sẽ là true.