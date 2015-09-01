{% import './data.html' as data %}

# Warmup

As warmup, let's being with some simple questions.

The comments data is imported as `data.comments`.

## How many people have submitted comments?

{% lodash %}
return _.size(data.comments);
{% endlodash %}

There are {{result}} submissions.

## Who is the first person submitting a comment?

We can get the data of the first comment by

{% lodash %}
return _.get(data.comments, 0)
{% endlodash %}

The result is

{{ result | json }}

The person's name is {{ result.user.login }}.

{% set name = result.user.login %}

## What is {{name}}'s favorite food?

We will need to parse the comment to retrieve this data.

The comment text is
{{ result.body | json }}

The code to retrieve the data about the favorite food is (hint: use [split()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/split)).

{% lodash %}
var text = _.last(_.last(_.first(data.comments).body.split('\n')).split(' '))

return text
{% endlodash %}

So, {{name}}'s favorite food is {{result}}.
