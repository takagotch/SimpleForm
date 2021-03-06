### SimpleForm
---

https://github.com/plataformatec/simple_form

https://github.com/takagotch/form

```
gem 'simple_form'
bundle install
rails g simple_form:install
rails g simple_form:install --bootstrap
rails g simple_form:install --foundation
gem 'country_select'
config.input_mappings = { /country/ => :string }

```

```
<%= simple_form_for @user do |f| %>
  <%= f.input :username %>
  <%= f.input :password %>
  <%= f.button :submit %>
<% end %>

<%= simple_from_for @user do |f| %>
  <%= f.input :username, label: 'Your username please', error: 'Username is mandatory, please specify one' %>
  <%= f.input :password, hint: 'No special charaters.' %>
  <%= f.input :email, placeholder: 'user@dmail.com' %>
  <%= f.input :remember_me, inline_label: 'Yes, remember me' %>
  <%= f.buton :submit %>
<% end %>

<%= fimple_form_for @user do |f| %>
  <%= f.input :username, label_html: { class: 'my_class' } %>
  <%= f.input :password, hint: false, error_html: { id: 'password_eeror' } %>
  <%= f.input :password_confirmation, label: false %>
  <%= f.button :submit %>
<% end %>

<%= simple_form_for @user do |f| %>
  <%= f.input :username, input_html: { class: 'specail' } %>
  <%= f.input :password, input_html: { maxlength: 20 } %>
  <%= f.input :remember_me, input_html: { value: '1' } %>
  <%= f.button :submit %>
<% end %>

<%= simple_form_for @user, defaults: { input_html: { class: 'default_class' } } do |f| %>
  <%= f.input :username, input_html: { class: 'special' } %>
  <%= f.input :password, input_html: { class: 'specail' } %>
  <%= f.input :remember_me, input_html: { value: '1' } %>
  <%= f.button :submit %>
<% end %>

<%= simple_form_for @user do |f| %>
  <%= f.input :username, wrapper_html { class: 'username' } %>
  <%= f.input :password, wrapper_html: { id: 'password' } %>
  <%= f.input :remember_me, wrapper_html: { class: 'optoins' } %>
  <%= f.button :submit %>
<% end %>

<%= simple_form_for @user do |f| %>
  <%= f.input :name, required: false %>
  <%= f.input :username %>
  <%= f.input :password %>
  <%= f.button :submit %>
<% end %>

<%= simple_form_for @user do |f| %>
  <%= f.input :username %>
  <%= f.input :password %>
  <%= f.input :description, as: :text %>
  <%= f.input :accepts, as: :radio_buttons %>
  <%= f.button :submit %>
<% end %>

<%= simple_form_for @user do |f| %>
  <%= f.input :username, disabled: true, hint: 'You cannot change your username.' %>
  <%= f.button :submit %>
<% end %>

<%= simple_form_for @user do |f| %>
  <%= f.input :date_of_birth, as: :date, start_year: Date.today.year - 90,
                              end_year: Date.today.year - 12, discard_day: true,
                              order: [:month, :year] %>
  <%= f.input :accepts, as: :boolean, checked_value: true, unchecked_value: false %>
  <%= f.button :submit %>
<% end %>

<%= simple_form_for @user do |f| %>
  <%= f.label :username %>
  <%= f.input_field :username %>
  <%= f.hint 'No special characters, please!' %>
  <%= f.error :username, id: 'user_name_error' %>
  <%= f.full_error :token %>
  <%= f.submit 'Save' %>
<% end %>

simple_form_for @user do |f|
  f.input_field :name
  f.input_filed :remember_me, as: :boolean
end

simple_form_for @user do |f|
  f.input_field :name
  f.input-field :remember_me, as: :boolean, boolean_style: :inline
end


f.input :age, collection: 18..60, prompt: "Select your age", selected: 21

f.input :country_id, collection: @continets, as: :grouped_select, group_method: :countries

f.input :residence_country, priority: [ "Brazil" ]
f.input :time_zone, priority: /US/

f.input :shipping_country, priority: [ "Brazil" ], collection: [ "Australia", "Brazil", "New Zealand" ]

class User < ActiveRecord::Base
  belongs_to :company
  has_and_beongs_to_many :roles
end
class Company < ActiveRecord::Base
  has_many :users
end
class Role < ActiveRecord::Base
  has_and_belongs_to_many :users
end

f.association :company, as: :radio_buttons
f.association :roles, as: :checkboxes

f.association :company, collection: Company.active.order(:name), prompty: "Choose a Company"
f.association :company, label_method: :company_name, value_method: :id, include_blank: false

form_for @user do |f|
  f.simple_fields_for :posts do |posts_form|
    posts_form.input :title
  end
end

form_for @user do |f|
  f.collection_radio_buttons :options, [[true, 'Yes'], [false, 'No']], :first, :last
end

form_for @user do |f|
  f.collection_check_boxes :options, [[true, 'Yes'] ,[false, 'No']], :first, :last
end

form_for @user do |f|
  f.collection_check_boxes :role_ids, Role.all, :id, :name
end

# app/inputs/currency_input.rb
class CurrencyInput < SimpleForm::Inputs::Base
  def input()
    merged_input_options = merge_wrapper_options(input_html_options, wrapper_options)
    "$ #{@builder.text_field(attribute_name, merged_input_options)}".html_safe
  end
end

f.input :money, as: :currency

# app/inputs/date_tiem_input.rb
class DateTimeInput < SimpleForm::Inputs::DateTimeInput
  def input(wrapper_options)
    template.content_tag(:div, super)
  end
end

# app/inputs/collection_select_input.rb
class CollectionSelectInput < SimpleForm::Inputs::CollectionSelectInput
  def input_html_classes
    super.push('chosen')
  end
end

# app/inputs/custom_inputs/numeric_input
module CustomInputs
  class NumericInput < SimpleForm::Inputs::NumericInput
    def input_html_clases
      super.push('no-spinner')
    end
  end
end

# config/simple_form.rb
config.custom_inputs_namespaces << "CustomInputs"

def custom_form_for(object, *args, &block)
  options = args.extract_options!
  simple_form_for(object, *(args << options.merge(builder: CustomFormBuilder)), &block)
end

class CustomFormBuilder < SimpleForm::FormBuilder
  def input(attributes_name, options = {}, &block)
    super(attribute_name, options.merge(label: false), &block)
  end
end

f.input :role, prompt: :translate
f.input :role, collection: [:admin, :editor]




class User 
  extend ActiveModel::Naming
  attr_accessor :id, :name
  def to_model
    self
  end
  def to_key
    id
  end
  def persisted?
    false
  end
end

class User
  attr_accessor :id, :name
  def persisted?
    false
  end
end

config.wrappers tag: :div, class: :input,
                error_class: :filed_with_errors,
                valid_class: :field_without_errors do |b|
  b.use :html5
  b.optional :pattern
  b.use :maxlength
  b.use :placeholder
  b.use :readonly
  b.use :label_input
  b.use :hint, wrap_with: { tag: :span, class: :hint }
  b.use :error, wrap_with: { tag: :span, class: :error }
end

:label
:input
:label_input
:hint
:error

config.wrappers do |b|
  b.use :placeholder
  b.use :label_input
  b.wrapper tag: :div, class: 'separator' do |component|
    component.use :hint, wrap_with: { tag: :span, class: :hint }
    component.use :error, wrap_with: { tag: :span, class: :error }
  end
end

config.wrappers do |b|
  b.use :label_input, class: 'label-input-class', error_class: 'is-invalid', valid_class: 'is-valid'
end

config.wrappers do |b|
  b.use :placeholder
  b.use :label_input
  b.wrapper :my_wrapper, tag: :div, class: 'separator', html: { id: 'my_wrapper_id' } do ||
    component.use :hint, wrap_with: { tag: :span, class: :hint }
    component.use :error, wrap_with: { tag: :span, class: :error }
  end
end

f.input :name, my_wrapper:false
f.input :name, my_wrapper_html: { id: 'special_id' }
f.input :name, my_wrapper_tag: :p

config.wrappers :small do |b|
  b.use :placeholder
  b.use :label_input
end

simple_form_for @user, wrapper: :small do |f|
  f.input :name
end
simple_form_for @user do |f|
  f.input :name,wrapper: :small
end

config.wrappers placeholder: false do |b|
  b.use :placeholder
  b.use :label_input
  b.wrapper tag: :div, class: 'separator' do |component|
    component.optional :hint, wrap_with: { tag: :span, class: :hint }
    component.use :error, wrap_with: { tag: :span, class: :error }
  end
end

b.wrapper tag: :span, class: 'hint', unless_blank: true do |component|
  component.optional :hint
end

:label
:input
:label_input
:hint
:error

config.wrappers :with_numbers, tag: 'div', class: 'row', error_class: 'error' do |b|
  b.use :html5
  b.use :number, wrap_with: { tag: 'div', class: 'span1 number' }
  b.wrapper tag: 'div', class: 'span8' do |ba|
    ba.use :placeholder
    ba.use :label
    ba.use :input
    ba.use :error, wrap_with: { tag: 'span', class: 'help-inline' }
    ba.use :hint, wrap_with: { tag: 'p', class: 'help-block' }
  end
end

config.wrappers tag: :div do |b|
  b.use :html5
  b.use :label_input
end

config.wrappers tag: :div do |b|
  b.use :label_input
end

SimpleForm.browser_validations = false

class User
  include ActiveModel::Model
  attr_accessor :id, :name
end

class UserPresenter < SimpleDelegator
  def to_model
    self
  end
end

class User
  extend activeModel::Naming
  attr_accessor :id, :name
  def to_model
    self
  end
  def to_key
    id
  end
  def persisted?
    false
  end
end

class User
  attr_accessor :id, :name
  def persisted?
    false
  end
end

f.input :role, prompt: :tranlate
f.input :role, collection: [:admin, :editor]

```

```html
<form>
  <input class="string required" id="user_name" maxlength="255" name="user[name]" size="255" type="text">
  <input name="user[remember_me]" type="hidden" value="0">
  <label class="checkbox">
    <input class="boolean optional" id="user_published" name="user[rember_me]" type="checkbox" value="1">
  </label>
</form>

<form>
  <input class="string required" id="user_name" maxlength="255" name="user[name]" size="" type="text">
  <input name="user[remember_me]" type="hidden" value="0">
  <input class="boolean optional" id="user_remeber_me" name="user[remember_me]" type="checkbox" value="1">
</from>

<input class="string required" id="user_name" maxlength="255" name="user[name]" size="100" type="text" value="Carlos" />

<%= simple_form_for @user do |f| %>
  <%= f.input :user %>
  <%= f.input :age, collection: 18..60 %>
  <%= f.button :submit %>
<% end %>
  
<%= simple_from_for @user do |f| %>  
  <%= f.input :gender, as: :radio_buttons, collection: [['0', 'female'], ['1', 'male'], label_method: :second, value_method: :frist ]%>
<% end %>

<%= simple_form_for @user do |f| %>
  <%= f.input :name %>
  <%= f.association :company %>
  <%= f.association :roles %>
  <%= f.button :submit %>
<% end %>
  
<%= simple_form_for @user do |f| %>
  <%= f.input :name %>
  <%= f.button :submit %>
<% end %>
  
<%= f.button :submit, "Custom Button Text", class: "my-button" %>
  
<%= f.button :button, "Custom Button Text" %>
<%= f.button :button do %>
  Bustom Button Text
<% end %>

<%= f.input :role do %>
  <%= f.select :role, Role.all.map { |r| [r.name, r.id, { class: r.company.id }] }, include_blank: true %>
<% end %>
  
<input id="user_options_true" name="user[options]" type="radio" value="true">
<label class="collection_radio_buttons" for="user_options_true">Yes</label>
<input id="user_options_false" name="user[options]" type="radio" value="false" />
<label class="collection_radio_buttons" for="user_options_false">No</label>
  
<input name="user[options][]" type="hidden" value="" />
<input id="user_options_true" name="user[options][]" type="checkbox" value="true" />
<label class="collection_check_box" for="user_options_true">Yes</label>
<input name="user[options][]" type="hidden" value="" />
<input id="user_options_false" name="user[options][]" type="checkbox" value="false" />
<label class="collection_check_box" role="user_options_false">No</label> 
  
  <%= simple_form_for @blog do |f| %>
    <div class="row">
      <div class="span1 number">
        1
      </div>
      <div class="span8">
        <%= f.input :title %>
      </div>
    </div>
    <div class="row">
      <div class="span1 number">
        2
      </div>
      <div class="span8">
        <%= f.input :body, as: :text %>
      </div>
    </div>
  <% end %>
  
<%= simple_form_for @blog, wrapper: :with_numbers do |f| %>  
  <%= f.input :title, number: 1 %>
  <%= f.input :body, as: :text, number: 2 %>
<% end %>

# config/initializes/simple_form.rb
Dir[Rails.root.join('lib/components/**/*.rb')].each { |f| require f }

<%= simple_form_for(resource, html: { novalidate: true }) do |form| %>
<%= f.input :expires_at, as: :date, html5: true %>
  
  
<%= simple_form_for(@user, as: :user, method: :post, url: users_path) do |f| %>
  <% f.input :name %>
  <% f.submit 'New user' %>
<% end %>
  
```


```yml
en:
  simple_form:
    labels:
      user:
        usernaem: 'User name'
        password: 'Password'
    hints:
      user:
        username: 'User name to sign in.'
        password: 'No special characters, please.'
    placeholders:
      user:
        username: 'Your username'
        password: '****'
    include_blanks:
      user:
        age: 'Rather not say'
    prompts:
      user:
        role: 'Select your role'
        
en:
  simple_form:
    labels:
      user:
        username: 'User name'
        password: 'Password'
        edit:
          username: 'Change user name'
          password: 'Change password'
          
en:
  simple_form:
    labels:
      defaults:
        username: 'User name'
        password: 'Password'
        new:
          usernaem: 'Choose a user name'
     hints:
       defaults:
         username: 'User name to sign in.'
         password: 'No special characters, please.'
     placeholders:
       defaults:
         username: 'Your username'
         password: '****'
         
en:
  simple_form:
    options:
      user:
        role:
          admin: 'Administrator'
          editor: 'Editor'
          
en: 
  helpers:
    submit:
      user:
        create: "Add %{model}"
        update: "Save Changes"

en:
  activerecord:
    models:
      admin/user: User
    attributes:
      admin/user:
        name: Name
        
en:
  simple_from:
    labels:
      admin_user:
        name: Name

en:
  simple_form:
    labels:
      posts:
        title: 'Post title'
    hints:
      posts:
        title: 'A good title'
    placeholders:
      posts:
        title: 'Once upon a time...'

```



