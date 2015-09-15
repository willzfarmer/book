{% data src="fcq.clean.json" %}
{% enddata %}

# Warmup

Next, complete the following warmup exercises as a team.

## How many unique subject codes?

{% lodash %}
return _.chain(data)
        .pluck("Subject")
        .uniq()
        .value()
        .length;
{% endlodash %}

They are {{ result }} unique subject codes.

## How many computer science (CSCI) courses?

{% lodash %}
return _.chain(data)
        .pluck("CrsPBADept")
        .filter(function(s) { return (s == "CSCI"); })
        .value()
        .length;
{% endlodash %}

They are {{ result }} computer science courses.

## What is the distribution of the courses across subject codes?

{% lodash %}
return _.chain(data)
        .pluck("Subject")
        .groupBy(function(s) { return s; })
        .mapValues(function(arr) { return arr.length; })
        .value();
{% endlodash %}

<table>
{% for key, value in result %}
    <tr>
        <td>{{key}}</td>
        <td>{{value}}</td>
    </tr>
{% endfor %}
</table>

## What subset of these subject codes have more than 100 courses?

{% lodash %}
return _.chain(data)
        .pluck("Subject")
        .groupBy(function(s) { return s; })
        .mapValues(function(arr) { return arr.length; })
        .pick(function(x) { return (x > 100); })
        .value();
{% endlodash %}

<table>
{% for key, value in result %}
    <tr>
        <td>{{key}}</td>
        <td>{{value}}</td>
    </tr>
{% endfor %}
</table>

## What subset of these subject codes have more than 5000 total enrollments?

{% lodash %}
return _.chain(data)
        .groupBy(function(obj) { return obj.Subject; })
        .mapValues(function(arr) {
            return _.chain(arr)
                    .map(function(obj) { return obj.N.ENROLL; })
                    .reduce(function(prev, next) { return prev + next; })
        }).pick(function(x) { return (x > 5000); })
        .value();
{% endlodash %}

<table>
{% for key, value in result %}
    <tr>
        <td>{{key}}</td>
        <td>{{value}}</td>
    </tr>
{% endfor %}
</table>

## What are the course numbers of the courses Tom (PEI HSIU) Yeh taught?

{% lodash %}
return _.chain(data)
        .filter(function(obj) {
            return _.reduce(obj.Instructors, function(prev, next, key) {
                        return (prev || (next.name == "YEH, PEI HSIU"));
                    }, false);
        }).map(function(obj) {
            return obj.Course;
        }).uniq()
        .value();
{% endlodash %}

They are {{result}}.
