.. highlightlang:: html+django
		   
Filters
=======

Overview
--------

You can modify variables for display by using **filters**.

Filters look like this: ``{{ name|lower }}``. This displays the value of the
``{{ name }}`` variable after being filtered through the :tfilter:`lower`
filter, which converts text to lowercase. Use a pipe (``|``) to apply a filter.

Filters can be "chained." The output of one filter is applied to the next.
``{{ text|escape|linebreaks }}`` is a common idiom for escaping text contents,
then converting line breaks to ``<p>`` tags.

Some filters take arguments. A filter argument looks like this: ``{{
bio|truncatewords:30 }}``. This will display the first 30 words of the ``bio``
variable.

Filter arguments that contain spaces must be quoted; for example, to join a
list with commas and spaced you'd use ``{{ list|join:", " }}``.

Djula provides about thirty built-in template filters. You can read all about
them in the :ref:`built-in filter reference <ref-templates-builtins-filters>`.
To give you a taste of what's available, here are some of the more commonly
used template filters:

List of filters
---------------

.. contents:: Filters
   :local:

.. templatefilter:: add

add
^^^

Adds the argument to the value.

For example::

    {{ value|add:"2" }}

If ``value`` is ``4``, then the output will be ``6``.

This filter will first try to coerce both values to integers. If this fails,
it'll attempt to add the values together anyway. This will work on some data
types (strings, list, etc.) and fail on others. If it fails, the result will
be an empty string.

For example, if we have::

    {{ first|add:second }}

and ``first`` is ``[1, 2, 3]`` and ``second`` is ``[4, 5, 6]``, then the
output will be ``[1, 2, 3, 4, 5, 6]``.

.. warning::

    Strings that can be coerced to integers will be **summed**, not
    concatenated, as in the first example above.

.. templatefilter:: addslashes

addslashes
^^^^^^^^^^

Adds slashes before quotes. Useful for escaping strings in CSV, for example.

For example::

    {{ value|addslashes }}

If ``value`` is ``"I'm using Django"``, the output will be
``"I\'m using Django"``.

.. templatefilter:: capfirst

capfirst
^^^^^^^^

Capitalizes the first character of the value. If the first character is not
a letter, this filter has no effect.

For example::

    {{ value|capfirst }}

If ``value`` is ``"django"``, the output will be ``"Django"``.

.. templatefilter:: center

center
^^^^^^

Centers the value in a field of a given width.

For example::

    "{{ value|center:"15" }}"

If ``value`` is ``"Django"``, the output will be ``"     Django    "``.

.. templatefilter:: cut

cut
^^^

Removes all values of arg from the given string.

For example::

    {{ value|cut:" " }}

If ``value`` is ``"String with spaces"``, the output will be
``"Stringwithspaces"``.

.. templatefilter:: date

date
^^^^

Formats a date according to the given format.

Uses a similar format as PHP's ``date()`` function (http://php.net/date)
with some differences.

.. note::
    These format characters are not used in Django outside of templates. They
    were designed to be compatible with PHP to ease transitioning for designers.

.. _date-and-time-formatting-specifiers:

Available format strings:

================  ========================================  =====================
Format character  Description                               Example output
================  ========================================  =====================
a                 ``'a.m.'`` or ``'p.m.'`` (Note that       ``'a.m.'``
                  this is slightly different than PHP's
                  output, because this includes periods
                  to match Associated Press style.)
A                 ``'AM'`` or ``'PM'``.                     ``'AM'``
b                 Month, textual, 3 letters, lowercase.     ``'jan'``
B                 Not implemented.
c                 ISO 8601 format. (Note: unlike others     ``2008-01-02T10:30:00.000123+02:00``,
                  formatters, such as "Z", "O" or "r",      or ``2008-01-02T10:30:00.000123`` if the datetime is naive
                  the "c" formatter will not add timezone
                  offset if value is a naive datetime
                  (see :class:`datetime.tzinfo`).
d                 Day of the month, 2 digits with           ``'01'`` to ``'31'``
                  leading zeros.
D                 Day of the week, textual, 3 letters.      ``'Fri'``
e                 Timezone name. Could be in any format,
                  or might return an empty string,          ``''``, ``'GMT'``, ``'-500'``, ``'US/Eastern'``, etc.
                  depending on the datetime.
E                 Month, locale specific alternative
                  representation usually used for long
                  date representation.                      ``'listopada'`` (for Polish locale, as opposed to ``'Listopad'``)
f                 Time, in 12-hour hours and minutes,       ``'1'``, ``'1:30'``
                  with minutes left off if they're zero.
                  Proprietary extension.
F                 Month, textual, long.                     ``'January'``
g                 Hour, 12-hour format without leading      ``'1'`` to ``'12'``
                  zeros.
G                 Hour, 24-hour format without leading      ``'0'`` to ``'23'``
                  zeros.
h                 Hour, 12-hour format.                     ``'01'`` to ``'12'``
H                 Hour, 24-hour format.                     ``'00'`` to ``'23'``
i                 Minutes.                                  ``'00'`` to ``'59'``
I                 Daylight Savings Time, whether it's       ``'1'`` or ``'0'``
                  in effect or not.
j                 Day of the month without leading          ``'1'`` to ``'31'``
                  zeros.
l                 Day of the week, textual, long.           ``'Friday'``
L                 Boolean for whether it's a leap year.     ``True`` or ``False``
m                 Month, 2 digits with leading zeros.       ``'01'`` to ``'12'``
M                 Month, textual, 3 letters.                ``'Jan'``
n                 Month without leading zeros.              ``'1'`` to ``'12'``
N                 Month abbreviation in Associated Press    ``'Jan.'``, ``'Feb.'``, ``'March'``, ``'May'``
                  style. Proprietary extension.
o                 ISO-8601 week-numbering year,             ``'1999'``
                  corresponding to
                  the ISO-8601 week number (W)
O                 Difference to Greenwich time in hours.    ``'+0200'``
P                 Time, in 12-hour hours, minutes and       ``'1 a.m.'``, ``'1:30 p.m.'``, ``'midnight'``, ``'noon'``, ``'12:30 p.m.'``
                  'a.m.'/'p.m.', with minutes left off
                  if they're zero and the special-case
                  strings 'midnight' and 'noon' if
                  appropriate. Proprietary extension.
r                 :rfc:`2822` formatted date.               ``'Thu, 21 Dec 2000 16:01:07 +0200'``
s                 Seconds, 2 digits with leading zeros.     ``'00'`` to ``'59'``
S                 English ordinal suffix for day of the     ``'st'``, ``'nd'``, ``'rd'`` or ``'th'``
                  month, 2 characters.
t                 Number of days in the given month.        ``28`` to ``31``
T                 Time zone of this machine.                ``'EST'``, ``'MDT'``
u                 Microseconds.                             ``000000`` to ``999999``
U                 Seconds since the Unix Epoch
                  (January 1 1970 00:00:00 UTC).
w                 Day of the week, digits without           ``'0'`` (Sunday) to ``'6'`` (Saturday)
                  leading zeros.
W                 ISO-8601 week number of year, with        ``1``, ``53``
                  weeks starting on Monday.
y                 Year, 2 digits.                           ``'99'``
Y                 Year, 4 digits.                           ``'1999'``
z                 Day of the year.                          ``0`` to ``365``
Z                 Time zone offset in seconds. The          ``-43200`` to ``43200``
                  offset for timezones west of UTC is
                  always negative, and for those east of
                  UTC is always positive.
================  ========================================  =====================

For example::

    {{ value|date:"D d M Y" }}

If ``value`` is a :py:class:`~datetime.datetime` object (e.g., the result of
``datetime.datetime.now()``), the output will be the string
``'Wed 09 Jan 2008'``.

The format passed can be one of the predefined ones :setting:`DATE_FORMAT`,
:setting:`DATETIME_FORMAT`, :setting:`SHORT_DATE_FORMAT` or
:setting:`SHORT_DATETIME_FORMAT`, or a custom format that uses the format
specifiers shown in the table above. Note that predefined formats may vary
depending on the current locale.

Assuming that :setting:`USE_L10N` is ``True`` and :setting:`LANGUAGE_CODE` is,
for example, ``"es"``, then for::

    {{ value|date:"SHORT_DATE_FORMAT" }}

the output would be the string ``"09/01/2008"`` (the ``"SHORT_DATE_FORMAT"``
format specifier for the ``es`` locale as shipped with Django is ``"d/m/Y"``).

When used without a format string::

    {{ value|date }}

...the formatting string defined in the :setting:`DATE_FORMAT` setting will be
used, without applying any localization.

You can combine ``date`` with the :tfilter:`time` filter to render a full
representation of a ``datetime`` value. E.g.::

    {{ value|date:"D d M Y" }} {{ value|time:"H:i" }}

.. templatefilter:: default

default
^^^^^^^

If value evaluates to ``False``, uses the given default. Otherwise, uses the
value.

For example::

    {{ value|default:"nothing" }}

If ``value`` is ``""`` (the empty string), the output will be ``nothing``.

.. templatefilter:: default_if_none

default_if_none
^^^^^^^^^^^^^^^

If (and only if) value is ``None``, uses the given default. Otherwise, uses the
value.

Note that if an empty string is given, the default value will *not* be used.
Use the :tfilter:`default` filter if you want to fallback for empty strings.

For example::

    {{ value|default_if_none:"nothing" }}

If ``value`` is ``None``, the output will be the string ``"nothing"``.

.. templatefilter:: dictsort

dictsort
^^^^^^^^

Takes a list of dictionaries and returns that list sorted by the key given in
the argument.

For example::

    {{ value|dictsort:"name" }}

If ``value`` is:

.. code-block:: python

    [
        {'name': 'zed', 'age': 19},
        {'name': 'amy', 'age': 22},
        {'name': 'joe', 'age': 31},
    ]

then the output would be:

.. code-block:: python

    [
        {'name': 'amy', 'age': 22},
        {'name': 'joe', 'age': 31},
        {'name': 'zed', 'age': 19},
    ]

You can also do more complicated things like::

    {% for book in books|dictsort:"author.age" %}
        * {{ book.title }} ({{ book.author.name }})
    {% endfor %}

If ``books`` is:

.. code-block:: python

    [
        {'title': '1984', 'author': {'name': 'George', 'age': 45}},
        {'title': 'Timequake', 'author': {'name': 'Kurt', 'age': 75}},
        {'title': 'Alice', 'author': {'name': 'Lewis', 'age': 33}},
    ]

then the output would be::

    * Alice (Lewis)
    * 1984 (George)
    * Timequake (Kurt)

.. templatefilter:: dictsortreversed

dictsortreversed
^^^^^^^^^^^^^^^^

Takes a list of dictionaries and returns that list sorted in reverse order by
the key given in the argument. This works exactly the same as the above filter,
but the returned value will be in reverse order.

.. templatefilter:: divisibleby

divisibleby
^^^^^^^^^^^

Returns ``True`` if the value is divisible by the argument.

For example::

    {{ value|divisibleby:"3" }}

If ``value`` is ``21``, the output would be ``True``.

.. templatefilter:: escape

escape
^^^^^^

Escapes a string's HTML. Specifically, it makes these replacements:

* ``<`` is converted to ``&lt;``
* ``>`` is converted to ``&gt;``
* ``'`` (single quote) is converted to ``&#39;``
* ``"`` (double quote) is converted to ``&quot;``
* ``&`` is converted to ``&amp;``

The escaping is only applied when the string is output, so it does not matter
where in a chained sequence of filters you put ``escape``: it will always be
applied as though it were the last filter. If you want escaping to be applied
immediately, use the :tfilter:`force_escape` filter.

Applying ``escape`` to a variable that would normally have auto-escaping
applied to the result will only result in one round of escaping being done. So
it is safe to use this function even in auto-escaping environments. If you want
multiple escaping passes to be applied, use the :tfilter:`force_escape` filter.

For example, you can apply ``escape`` to fields when :ttag:`autoescape` is off::

    {% autoescape off %}
        {{ title|escape }}
    {% endautoescape %}

.. templatefilter:: escapejs

escapejs
^^^^^^^^

Escapes characters for use in JavaScript strings. This does *not* make the
string safe for use in HTML, but does protect you from syntax errors when using
templates to generate JavaScript/JSON.

For example::

    {{ value|escapejs }}

If ``value`` is ``"testing\r\njavascript \'string" <b>escaping</b>"``,
the output will be ``"testing\\u000D\\u000Ajavascript \\u0027string\\u0022 \\u003Cb\\u003Eescaping\\u003C/b\\u003E"``.

.. templatefilter:: filesizeformat

filesizeformat
^^^^^^^^^^^^^^

Formats the value like a 'human-readable' file size (i.e. ``'13 KB'``,
``'4.1 MB'``, ``'102 bytes'``, etc).

For example::

    {{ value|filesizeformat }}

If ``value`` is 123456789, the output would be ``117.7 MB``.

.. admonition:: File sizes and SI units

    Strictly speaking, ``filesizeformat`` does not conform to the International
    System of Units which recommends using KiB, MiB, GiB, etc. when byte sizes
    are calculated in powers of 1024 (which is the case here). Instead, Django
    uses traditional unit names (KB, MB, GB, etc.) corresponding to names that
    are more commonly used.

.. templatefilter:: first

first
^^^^^

Returns the first item in a list.

For example::

    {{ value|first }}

If ``value`` is the list ``['a', 'b', 'c']``, the output will be ``'a'``.

.. templatefilter:: floatformat

floatformat
^^^^^^^^^^^

When used without an argument, rounds a floating-point number to one decimal
place -- but only if there's a decimal part to be displayed. For example:

============  ===========================  ========
``value``     Template                     Output
============  ===========================  ========
``34.23234``  ``{{ value|floatformat }}``  ``34.2``
``34.00000``  ``{{ value|floatformat }}``  ``34``
``34.26000``  ``{{ value|floatformat }}``  ``34.3``
============  ===========================  ========

If used with a numeric integer argument, ``floatformat`` rounds a number to
that many decimal places. For example:

============  =============================  ==========
``value``     Template                       Output
============  =============================  ==========
``34.23234``  ``{{ value|floatformat:3 }}``  ``34.232``
``34.00000``  ``{{ value|floatformat:3 }}``  ``34.000``
``34.26000``  ``{{ value|floatformat:3 }}``  ``34.260``
============  =============================  ==========

Particularly useful is passing 0 (zero) as the argument which will round the
float to the nearest integer.

============  ================================  ==========
``value``     Template                          Output
============  ================================  ==========
``34.23234``  ``{{ value|floatformat:"0" }}``   ``34``
``34.00000``  ``{{ value|floatformat:"0" }}``   ``34``
``39.56000``  ``{{ value|floatformat:"0" }}``   ``40``
============  ================================  ==========

If the argument passed to ``floatformat`` is negative, it will round a number
to that many decimal places -- but only if there's a decimal part to be
displayed. For example:

============  ================================  ==========
``value``     Template                          Output
============  ================================  ==========
``34.23234``  ``{{ value|floatformat:"-3" }}``  ``34.232``
``34.00000``  ``{{ value|floatformat:"-3" }}``  ``34``
``34.26000``  ``{{ value|floatformat:"-3" }}``  ``34.260``
============  ================================  ==========

Using ``floatformat`` with no argument is equivalent to using ``floatformat``
with an argument of ``-1``.

.. templatefilter:: force_escape

force_escape
^^^^^^^^^^^^

Applies HTML escaping to a string (see the :tfilter:`escape` filter for
details). This filter is applied *immediately* and returns a new, escaped
string. This is useful in the rare cases where you need multiple escaping or
want to apply other filters to the escaped results. Normally, you want to use
the :tfilter:`escape` filter.

For example, if you want to catch the ``<p>`` HTML elements created by
the :tfilter:`linebreaks` filter::

    {% autoescape off %}
        {{ body|linebreaks|force_escape }}
    {% endautoescape %}

.. templatefilter:: get_digit

get_digit
^^^^^^^^^

Given a whole number, returns the requested digit, where 1 is the right-most
digit, 2 is the second-right-most digit, etc. Returns the original value for
invalid input (if input or argument is not an integer, or if argument is less
than 1). Otherwise, output is always an integer.

For example::

    {{ value|get_digit:"2" }}

If ``value`` is ``123456789``, the output will be ``8``.

.. templatefilter:: iriencode

iriencode
^^^^^^^^^

Converts an IRI (Internationalized Resource Identifier) to a string that is
suitable for including in a URL. This is necessary if you're trying to use
strings containing non-ASCII characters in a URL.

It's safe to use this filter on a string that has already gone through the
:tfilter:`urlencode` filter.

For example::

    {{ value|iriencode }}

If ``value`` is ``"?test=1&me=2"``, the output will be ``"?test=1&amp;me=2"``.

.. templatefilter:: join

join
^^^^

Joins a list with a string, like Python's ``str.join(list)``

For example::

    {{ value|join:" // " }}

If ``value`` is the list ``['a', 'b', 'c']``, the output will be the string
``"a // b // c"``.

.. templatefilter:: last

last
^^^^

Returns the last item in a list.

For example::

    {{ value|last }}

If ``value`` is the list ``['a', 'b', 'c', 'd']``, the output will be the
string ``"d"``.

.. templatefilter:: length

length
^^^^^^

Returns the length of the value. This works for both strings and lists.

For example::

    {{ value|length }}

If ``value`` is ``['a', 'b', 'c', 'd']`` or ``"abcd"``, the output will be
``4``.

.. versionchanged:: 1.8

    The filter returns ``0`` for an undefined variable. Previously, it returned
    an empty string.

.. templatefilter:: length_is

length_is
^^^^^^^^^

Returns ``True`` if the value's length is the argument, or ``False`` otherwise.

For example::

    {{ value|length_is:"4" }}

If ``value`` is ``['a', 'b', 'c', 'd']`` or ``"abcd"``, the output will be
``True``.

.. templatefilter:: linebreaks

linebreaks
^^^^^^^^^^

Replaces line breaks in plain text with appropriate HTML; a single
newline becomes an HTML line break (``<br />``) and a new line
followed by a blank line becomes a paragraph break (``</p>``).

For example::

    {{ value|linebreaks }}

If ``value`` is ``Joel\nis a slug``, the output will be ``<p>Joel<br />is a
slug</p>``.

.. templatefilter:: linebreaksbr

linebreaksbr
^^^^^^^^^^^^

Converts all newlines in a piece of plain text to HTML line breaks
(``<br />``).

For example::

    {{ value|linebreaksbr }}

If ``value`` is ``Joel\nis a slug``, the output will be ``Joel<br />is a
slug``.

.. templatefilter:: linenumbers

linenumbers
^^^^^^^^^^^

Displays text with line numbers.

For example::

    {{ value|linenumbers }}

If ``value`` is::

    one
    two
    three

the output will be::

    1. one
    2. two
    3. three

.. templatefilter:: ljust

ljust
^^^^^

Left-aligns the value in a field of a given width.

**Argument:** field size

For example::

    "{{ value|ljust:"10" }}"

If ``value`` is ``Django``, the output will be ``"Django    "``.

.. templatefilter:: lower

lower
^^^^^

Converts a string into all lowercase.

For example::

    {{ value|lower }}

If ``value`` is ``Still MAD At Yoko``, the output will be
``still mad at yoko``.

.. templatefilter:: make_list

make_list
^^^^^^^^^

Returns the value turned into a list. For a string, it's a list of characters.
For an integer, the argument is cast into an unicode string before creating a
list.

For example::

    {{ value|make_list }}

If ``value`` is the string ``"Joel"``, the output would be the list
``['J', 'o', 'e', 'l']``. If ``value`` is ``123``, the output will be the
list ``['1', '2', '3']``.

.. templatefilter:: phone2numeric

phone2numeric
^^^^^^^^^^^^^

Converts a phone number (possibly containing letters) to its numerical
equivalent.

The input doesn't have to be a valid phone number. This will happily convert
any string.

For example::

    {{ value|phone2numeric }}

If ``value`` is ``800-COLLECT``, the output will be ``800-2655328``.

.. templatefilter:: pluralize

pluralize
^^^^^^^^^

Returns a plural suffix if the value is not 1. By default, this suffix is
``'s'``.

Example::

    You have {{ num_messages }} message{{ num_messages|pluralize }}.

If ``num_messages`` is ``1``, the output will be ``You have 1 message.``
If ``num_messages`` is ``2``  the output will be ``You have 2 messages.``

For words that require a suffix other than ``'s'``, you can provide an alternate
suffix as a parameter to the filter.

Example::

    You have {{ num_walruses }} walrus{{ num_walruses|pluralize:"es" }}.

For words that don't pluralize by simple suffix, you can specify both a
singular and plural suffix, separated by a comma.

Example::

    You have {{ num_cherries }} cherr{{ num_cherries|pluralize:"y,ies" }}.

.. note:: Use :ttag:`blocktrans` to pluralize translated strings.

.. templatefilter:: pprint

pprint
^^^^^^

A wrapper around :func:`pprint.pprint` -- for debugging, really.

.. templatefilter:: random

random
^^^^^^

Returns a random item from the given list.

For example::

    {{ value|random }}

If ``value`` is the list ``['a', 'b', 'c', 'd']``, the output could be ``"b"``.

.. templatefilter:: removetags

removetags
^^^^^^^^^^

Removes a space-separated list of [X]HTML tags from the output.

For example::

    {{ value|removetags:"b span"|safe }}

If ``value`` is ``"<b>Joel</b> <button>is</button> a <span>slug</span>"`` the
output will be ``"Joel <button>is</button> a slug"``.

Note that this filter is case-sensitive.

If ``value`` is ``"<B>Joel</B> <button>is</button> a <span>slug</span>"`` the
output will be ``"<B>Joel</B> <button>is</button> a slug"``.

.. templatefilter:: rjust

rjust
^^^^^

Right-aligns the value in a field of a given width.

**Argument:** field size

For example::

    "{{ value|rjust:"10" }}"

If ``value`` is ``Django``, the output will be ``"    Django"``.

.. templatefilter:: safe

safe
^^^^

Marks a string as not requiring further HTML escaping prior to output. When
autoescaping is off, this filter has no effect.

.. note::

    If you are chaining filters, a filter applied after ``safe`` can
    make the contents unsafe again. For example, the following code
    prints the variable as is, unescaped:

    .. code-block:: html+django

        {{ var|safe|escape }}

.. templatefilter:: safeseq

safeseq
^^^^^^^

Applies the :tfilter:`safe` filter to each element of a sequence. Useful in
conjunction with other filters that operate on sequences, such as
:tfilter:`join`. For example::

    {{ some_list|safeseq|join:", " }}

You couldn't use the :tfilter:`safe` filter directly in this case, as it would
first convert the variable into a string, rather than working with the
individual elements of the sequence.

.. templatefilter:: slice

slice
^^^^^

Returns a slice of the list.

Uses the same syntax as Python's list slicing. See
http://www.diveintopython3.net/native-datatypes.html#slicinglists
for an introduction.

Example::

    {{ some_list|slice:":2" }}

If ``some_list`` is ``['a', 'b', 'c']``, the output will be ``['a', 'b']``.

.. templatefilter:: slugify

slugify
^^^^^^^

Converts to lowercase, removes non-word characters (alphanumerics and
underscores) and converts spaces to hyphens. Also strips leading and trailing
whitespace.

For example::

    {{ value|slugify }}

If ``value`` is ``"Joel is a slug"``, the output will be ``"joel-is-a-slug"``.

.. templatefilter:: stringformat

stringformat
^^^^^^^^^^^^

Formats the variable according to the argument, a string formatting specifier.
This specifier uses Python string formatting syntax, with the exception that
the leading "%" is dropped.

See http://docs.python.org/library/stdtypes.html#string-formatting-operations
for documentation of Python string formatting

For example::

    {{ value|stringformat:"E" }}

If ``value`` is ``10``, the output will be ``1.000000E+01``.

.. templatefilter:: striptags

striptags
^^^^^^^^^

Makes all possible efforts to strip all [X]HTML tags.

For example::

    {{ value|striptags }}

If ``value`` is ``"<b>Joel</b> <button>is</button> a <span>slug</span>"``, the
output will be ``"Joel is a slug"``.

.. admonition:: No safety guarantee

    Note that ``striptags`` doesn't give any guarantee about its output being
    entirely HTML safe, particularly with non valid HTML input. So **NEVER**
    apply the ``safe`` filter to a ``striptags`` output.
    If you are looking for something more robust, you can use the ``bleach``
    Python library, notably its `clean`_ method.

.. _clean: http://bleach.readthedocs.org/en/latest/clean.html

.. templatefilter:: time

time
^^^^

Formats a time according to the given format.

Given format can be the predefined one :setting:`TIME_FORMAT`, or a custom
format, same as the :tfilter:`date` filter. Note that the predefined format
is locale-dependent.

For example::

    {{ value|time:"H:i" }}

If ``value`` is equivalent to ``datetime.datetime.now()``, the output will be
the string ``"01:23"``.

Another example:

Assuming that :setting:`USE_L10N` is ``True`` and :setting:`LANGUAGE_CODE` is,
for example, ``"de"``, then for::

    {{ value|time:"TIME_FORMAT" }}

the output will be the string ``"01:23:00"`` (The ``"TIME_FORMAT"`` format
specifier for the ``de`` locale as shipped with Django is ``"H:i:s"``).

The ``time`` filter will only accept parameters in the format string that
relate to the time of day, not the date (for obvious reasons). If you need to
format a ``date`` value, use the :tfilter:`date` filter instead (or along
``time`` if you need to render a full :py:class:`~datetime.datetime` value).

There is one exception the above rule: When passed a ``datetime`` value with
attached timezone information (a :ref:`time-zone-aware
<naive_vs_aware_datetimes>` ``datetime`` instance) the ``time`` filter will
accept the timezone-related :ref:`format specifiers
<date-and-time-formatting-specifiers>` ``'e'``, ``'O'`` , ``'T'`` and ``'Z'``.

When used without a format string::

    {{ value|time }}

...the formatting string defined in the :setting:`TIME_FORMAT` setting will be
used, without applying any localization.

.. versionchanged:: 1.7

    The ability to receive and act on values with attached timezone
    information was added in Django 1.7.

.. templatefilter:: timesince

timesince
^^^^^^^^^

Formats a date as the time since that date (e.g., "4 days, 6 hours").

Takes an optional argument that is a variable containing the date to use as
the comparison point (without the argument, the comparison point is *now*).
For example, if ``blog_date`` is a date instance representing midnight on 1
June 2006, and ``comment_date`` is a date instance for 08:00 on 1 June 2006,
then the following would return "8 hours"::

    {{ blog_date|timesince:comment_date }}

Comparing offset-naive and offset-aware datetimes will return an empty string.

Minutes is the smallest unit used, and "0 minutes" will be returned for any
date that is in the future relative to the comparison point.

.. templatefilter:: timeuntil

timeuntil
^^^^^^^^^

Similar to ``timesince``, except that it measures the time from now until the
given date or datetime. For example, if today is 1 June 2006 and
``conference_date`` is a date instance holding 29 June 2006, then
``{{ conference_date|timeuntil }}`` will return "4 weeks".

Takes an optional argument that is a variable containing the date to use as
the comparison point (instead of *now*). If ``from_date`` contains 22 June
2006, then the following will return "1 week"::

    {{ conference_date|timeuntil:from_date }}

Comparing offset-naive and offset-aware datetimes will return an empty string.

Minutes is the smallest unit used, and "0 minutes" will be returned for any
date that is in the past relative to the comparison point.

.. templatefilter:: title

title
^^^^^

Converts a string into titlecase by making words start with an uppercase
character and the remaining characters lowercase. This tag makes no effort to
keep "trivial words" in lowercase.

For example::

    {{ value|title }}

If ``value`` is ``"my FIRST post"``, the output will be ``"My First Post"``.

.. templatefilter:: truncatechars

truncatechars
^^^^^^^^^^^^^

Truncates a string if it is longer than the specified number of characters.
Truncated strings will end with a translatable ellipsis sequence ("...").

**Argument:** Number of characters to truncate to

For example::

    {{ value|truncatechars:9 }}

If ``value`` is ``"Joel is a slug"``, the output will be ``"Joel i..."``.

.. templatefilter:: truncatechars_html

truncatechars_html
^^^^^^^^^^^^^^^^^^

.. versionadded:: 1.7

Similar to :tfilter:`truncatechars`, except that it is aware of HTML tags. Any
tags that are opened in the string and not closed before the truncation point
are closed immediately after the truncation.

For example::

    {{ value|truncatechars_html:9 }}

If ``value`` is ``"<p>Joel is a slug</p>"``, the output will be
``"<p>Joel i...</p>"``.

Newlines in the HTML content will be preserved.

.. templatefilter:: truncatewords

truncatewords
^^^^^^^^^^^^^

Truncates a string after a certain number of words.

**Argument:** Number of words to truncate after

For example::

    {{ value|truncatewords:2 }}

If ``value`` is ``"Joel is a slug"``, the output will be ``"Joel is ..."``.

Newlines within the string will be removed.

.. templatefilter:: truncatewords_html

truncatewords_html
^^^^^^^^^^^^^^^^^^

Similar to :tfilter:`truncatewords`, except that it is aware of HTML tags. Any
tags that are opened in the string and not closed before the truncation point,
are closed immediately after the truncation.

This is less efficient than :tfilter:`truncatewords`, so should only be used
when it is being passed HTML text.

For example::

    {{ value|truncatewords_html:2 }}

If ``value`` is ``"<p>Joel is a slug</p>"``, the output will be
``"<p>Joel is ...</p>"``.

Newlines in the HTML content will be preserved.

.. templatefilter:: unordered_list

unordered_list
^^^^^^^^^^^^^^

Recursively takes a self-nested list and returns an HTML unordered list --
WITHOUT opening and closing <ul> tags.

The list is assumed to be in the proper format. For example, if ``var``
contains ``['States', ['Kansas', ['Lawrence', 'Topeka'], 'Illinois']]``, then
``{{ var|unordered_list }}`` would return::

    <li>States
    <ul>
            <li>Kansas
            <ul>
                    <li>Lawrence</li>
                    <li>Topeka</li>
            </ul>
            </li>
            <li>Illinois</li>
    </ul>
    </li>

Note: An older, more restrictive and verbose input format is also supported:
``['States', [['Kansas', [['Lawrence', []], ['Topeka', []]]], ['Illinois', []]]]``,

.. templatefilter:: upper

upper
^^^^^

Converts a string into all uppercase.

For example::

    {{ value|upper }}

If ``value`` is ``"Joel is a slug"``, the output will be ``"JOEL IS A SLUG"``.

.. templatefilter:: urlencode

urlencode
^^^^^^^^^

Escapes a value for use in a URL.

For example::

    {{ value|urlencode }}

If ``value`` is ``"http://www.example.org/foo?a=b&c=d"``, the output will be
``"http%3A//www.example.org/foo%3Fa%3Db%26c%3Dd"``.

An optional argument containing the characters which should not be escaped can
be provided.

If not provided, the '/' character is assumed safe. An empty string can be
provided when *all* characters should be escaped. For example::

    {{ value|urlencode:"" }}

If ``value`` is ``"http://www.example.org/"``, the output will be
``"http%3A%2F%2Fwww.example.org%2F"``.

.. templatefilter:: urlize

urlize
^^^^^^

Converts URLs and email addresses in text into clickable links.

This template tag works on links prefixed with ``http://``, ``https://``, or
``www.``. For example, ``http://goo.gl/aia1t`` will get converted but
``goo.gl/aia1t`` won't.

It also supports domain-only links ending in one of the original top level
domains (``.com``, ``.edu``, ``.gov``, ``.int``, ``.mil``, ``.net``, and
``.org``). For example, ``djangoproject.com`` gets converted.

.. versionchanged:: 1.8

    Support for domain-only links that include characters after the top-level
    domain (e.g. ``djangoproject.com/`` and ``djangoproject.com/download/``)
    was added.

Links can have trailing punctuation (periods, commas, close-parens) and leading
punctuation (opening parens), and ``urlize`` will still do the right thing.

Links generated by ``urlize`` have a ``rel="nofollow"`` attribute added
to them.

For example::

    {{ value|urlize }}

If ``value`` is ``"Check out www.djangoproject.com"``, the output will be
``"Check out <a href="http://www.djangoproject.com"
rel="nofollow">www.djangoproject.com</a>"``.

In addition to web links, ``urlize`` also converts email addresses into
``mailto:`` links. If ``value`` is
``"Send questions to foo@example.com"``, the output will be
``"Send questions to <a href="mailto:foo@example.com">foo@example</a>"``.

The ``urlize`` filter also takes an optional parameter ``autoescape``. If
``autoescape`` is ``True``, the link text and URLs will be escaped using
Django's built-in :tfilter:`escape` filter. The default value for
``autoescape`` is ``True``.

.. note::

    If ``urlize`` is applied to text that already contains HTML markup,
    things won't work as expected. Apply this filter only to plain text.

.. templatefilter:: urlizetrunc

urlizetrunc
^^^^^^^^^^^

Converts URLs and email addresses into clickable links just like urlize_, but truncates URLs
longer than the given character limit.

**Argument:** Number of characters that link text should be truncated to,
including the ellipsis that's added if truncation is necessary.

For example::

    {{ value|urlizetrunc:15 }}

If ``value`` is ``"Check out www.djangoproject.com"``, the output would be
``'Check out <a href="http://www.djangoproject.com"
rel="nofollow">www.djangopr...</a>'``.

As with urlize_, this filter should only be applied to plain text.

.. templatefilter:: wordcount

wordcount
^^^^^^^^^

Returns the number of words.

For example::

    {{ value|wordcount }}

If ``value`` is ``"Joel is a slug"``, the output will be ``4``.

.. templatefilter:: wordwrap

wordwrap
^^^^^^^^

Wraps words at specified line length.

**Argument:** number of characters at which to wrap the text

For example::

    {{ value|wordwrap:5 }}

If ``value`` is ``Joel is a slug``, the output would be::

    Joel
    is a
    slug

.. templatefilter:: yesno

yesno
^^^^^

Maps values for true, false and (optionally) None, to the strings "yes", "no",
"maybe", or a custom mapping passed as a comma-separated list, and
returns one of those strings according to the value:

For example::

    {{ value|yesno:"yeah,no,maybe" }}

==========  ======================  ==================================
Value       Argument                Outputs
==========  ======================  ==================================
``True``                            ``yes``
``True``    ``"yeah,no,maybe"``     ``yeah``
``False``   ``"yeah,no,maybe"``     ``no``
``None``    ``"yeah,no,maybe"``     ``maybe``
``None``    ``"yeah,no"``           ``"no"`` (converts None to False
                                    if no mapping for None is given)
==========  ======================  ==================================

Internationalization tags and filters
-------------------------------------

Django provides template tags and filters to control each aspect of
:doc:`internationalization </topics/i18n/index>` in templates. They allow for
granular control of translations, formatting, and time zone conversions.

i18n
^^^^

This library allows specifying translatable text in templates.
To enable it, set :setting:`USE_I18N` to ``True``, then load it with
``{% load i18n %}``.

See :ref:`specifying-translation-strings-in-template-code`.

l10n
^^^^

This library provides control over the localization of values in templates.
You only need to load the library using ``{% load l10n %}``, but you'll often
set :setting:`USE_L10N` to ``True`` so that localization is active by default.

See :ref:`topic-l10n-templates`.

tz
^^

This library provides control over time zone conversions in templates.
Like ``l10n``, you only need to load the library using ``{% load tz %}``,
but you'll usually also set :setting:`USE_TZ` to ``True`` so that conversion
to local time happens by default.

See :ref:`time-zones-in-templates`.