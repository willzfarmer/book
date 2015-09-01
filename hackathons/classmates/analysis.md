# Analysis

{% import './data.html' as data %}

After completing the warmup exercises, your task is to do four more slightly
more challenges analyses.

## How many students like sushi as their favorite food?

{% lodash %}
return _.size(_.groupBy(_.map(_.pluck(data.comments, 'body'), function(str) {
    var bodyArray = str.split('\n');
    var n = _.size(bodyArray) - 1
    if (bodyArray[n] != ""){
        var retval = bodyArray[n];
    } else {
        var retval = bodyArray[n - 1];
    }
    return _.last(retval.toLowerCase().split(':')).match(/[a-z][a-z ]*/)[0];
}), function(str) {
    return str;
})['sushi']);
{% endlodash %}

The answer is {{result}}.

## Who are the students liking Python the most?

{% lodash %}
return _.map(
    _.groupBy(
        _.map(data.comments, function(obj) {
            return [obj.user.login,
                obj.body.split('\n')[2].split(':')[1].toLowerCase().match(/[a-z]+/)[0]];
        }), function(arr) { return arr[1]; })['python'], function(arr) { return arr[0]; });
{% endlodash %}

Their names are {{result | json}}.

## Are there more Javascript lovers or Java lovers?

{% lodash %}
var languages =
    _.groupBy(
        _.map(
            data.comments, function(obj) {
                return [obj.user.login,
                    obj.body.split('\n')[2].split(':')[1].toLowerCase().match(/[a-z]+/)[0]];
            }),
     function(arr) {
         return arr[1];
     });

var java = _.size(languages['java']);
var javascript = _.size(languages['javascript']);

if (java > javascript) {
    return "Java";
} else if (javascript > java) {
    return "Javascript";
} else {
    return "Equal";
}
{% endlodash %}

The answer is {{result}}.

## Who like the same food as `kjblakemore`?

{% lodash %}
var foodArr =
    _.map(data.comments, function(obj) {
        return [obj.user.login,
                obj.body.toLowerCase().match(/food.*/)[0].match(/[a-z][a-z !,]*$/)[0]
            ];
    });

var foods = {};
for (var i = 0; i < foodArr.length; i++) {
    foods[foodArr[i][0]] = foodArr[i][1];
}

return _.map(_.groupBy(foodArr, function(tup) {
    return tup[1];
})[foods['kjblakemore']], function(tup) { return tup[0]; });
{% endlodash %}

Their names are {{result | json}}.
