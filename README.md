# RailsAudit

Unlike [audited](https://github.com/collectiveidea/audited) and [paper_trail](https://github.com/airblade/paper_trail) etc. These model audit tools use model callbacks to record every changes.

`rails_audit` record ActiveRecord Model changes in Controllers, it will record context with model changes:

1. controller
2. action
3. request.remote_ip
4. current_operator
5. any other info you want to record..

### Model style audit VS controller style audit

| Model Style Audit | Controller style Audit |
| --- | --- |
| Record every changes | Record only when you marked |
| use model callback, can skip by the data persistence not commit callback | No model callback |
| use thread variables delivery state from controller to model | Just add variables you can get in controller |

## Usage

### in Model
1. include Auditable

```ruby
class User < ActiveRecord::Base
  include Auditable
  
end

```

### in Controller

```ruby
class UsersController < ApplicationController
  
  # use after action, will auto record changes by use saved_changes api
  after_action only: [:update, :create, :destroy] do
    mark_audites(User, note: 'note something!', extra: { client_headers: request.headers })
  end
  
end
```

## License
The gem is available as open source under the terms of the [LGPL License](https://opensource.org/licenses/LGPL-3.0).