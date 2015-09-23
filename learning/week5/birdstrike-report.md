{% data src="birdstrike.json" %}
{% enddata %}

# Report

As a team, answer a subset of the questions submitted during the hackathon.
But instead of using Tableau, you will need to write Javascript/Lodash code
to derive your answers. Similar to before, each team member is responsible for
one question. But everyone should work together to come up with a good solution.
Your answer should consist of Lodash code and a brief writeup.
Utilize `_.map`, `_.filter`, `_.group` ...etc. Do not se any for loop.

This time, the data is not already prepared for you in a nice JSON format. You
will need to do it on your own, replacing the placeholder `birdstrike.json` with
real data.

I exported the data to CSV from LibreOffice and then used

    csvcut -c 'Airport: Name','Aircraft: Airline/Operator','Aircraft: Make/Model','Effect: Indicated Damage','When: Phase of flight','Feet above ground','Conditions: Sky','Cost: Total $' birdstrike.csv | csvjson --indent 4 > birdstrike.json

With resulting data of the format

{% lodash %}
console.log(data[0])
{% endlodash %}

# what are the condition:sky where usually caused a damage? - by fadhilfath

{% lodash %}
return _.chain(data)
        .groupBy(function(obj) { return obj["Conditions: Sky"]; })
        .mapValues(function(arr) { return arr.length; })
        .value();
{% endlodash %}

{{ result | json }}

# Kevin Gifford: Flight Phase of Birdstrikes:

{% lodash %}
return _.chain(data)
        .groupBy(function(obj) { return obj["When: Phase of flight"]; })
        .mapValues(function(arr) { return arr.length; })
        .value();
{% endlodash %}

{{ result | json }}

# What is the correlation between the feet above the ground and the number of bird strikes

{% lodash %}
return _.chain(data)
        .groupBy(function(obj) { return obj["Feet above ground"]; })
        .mapValues(function(arr) { return arr.length; })
        .value()
{% endlodash %}

{{ result | json }}

# Which airport had the highest number of bird strikes? (Zhya215).

{% lodash %}
return _.chain(data)
        .groupBy(function(obj) { return obj["Airport: Name"]; })
        .values()
        .map(function(arr) { return [arr[0]["Airport: Name"], arr.length]; })
        .reduce(function(n, p) {
            if ((n[1] > p[1]) && (n[0] != "UNKNOWN")) { return n; } else { return p; }
        }).value()
{% endlodash %}

{{ result | json }}

# Which airline have to incur most repair cost due to damage ? (sumi6109)

{% lodash %}
return _.chain(data)
        .groupBy(function(obj) { return obj["Aircraft: Airline/Operator"]; })
        .values()
        .map(function(arr) { return [arr[0]["Aircraft: Airline/Operator"], arr.length]; })
        .reduce(function(n, p) {
            if ((n[1] > p[1]) && (n[0] != "UNKNOWN")) { return n; } else { return p; }
        }).value()
{% endlodash %}

{{ result | json }}

# Which plane model strikes the most birds? (twagar95)

{% lodash %}
return _.chain(data)
        .groupBy(function(obj) { return obj["Aircraft: Make/Model"]; })
        .values()
        .map(function(arr) { return [arr[0]["Aircraft: Make/Model"], arr.length]; })
        .reduce(function(n, p) {
            if ((n[1] > p[1]) && (n[0] != "UNKNOWN")) { return n; } else { return p; }
        }).value()
{% endlodash %}

{{ result | json }}
