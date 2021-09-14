# Debian Vulnerability Investigation

Currently, (September 2021) Clair ingests Debian's [OVAL feeds][ovalfeed].
This allows us to reuse code between different distribution's feeds, as OVAL is relatively standardized.
However, in some cases the feed will have nonsensical version numbers (see: CVE-2016-6309, CVE-2010-4051).

I asked on IRC about this state of affairs:

><that_guy> I've got a question regarding the OVAL and json data feeds: some entries have a "fixed version" of "0" (e.g. CVE-2016-6309)
>
><that_guy> is this a bug in the feed generation? if not, how should it be interpreted?
>
><pabs> that_guy: that seems to come up often, last thread was https://lists.debian.org/msgid-search/CACoSkAMozE+58y7VyZe3oYZSyVops7EjxCBG6q=FjRit8SXe3A@mail.gmail.com
>
><pabs> sounds like this is a long-standing bug that needs help fixing https://lists.debian.org/msgid-search/CAB9B7UupNfMoGbnOzu6eRjvUZT2iDBbFfxEQ2sYnnW5bJ76rNQ@mail.gmail.com
>
><that_guy> pabs: interesting. The script linked in that last email seems to work off the json file, but I can't find where that's created. The issues seem to exist there, so I'm assuming the script that generates the OVAL XML is just inheriting that problem
>
><that_guy> Any pointers on where/how that's generated would be helpful

Following the linked mailing list thread leads one to [this][ovalscript] script.
This generates the OVAL XML, but uses the [JSON file][jsonfeed] to do so.
There does not seem to be a script in that repository to generate the JSON file.

Poking around on the Debian Gitlab instance turned up [this][security-tracker] repository that _seems_ to be the master source of vulnerability information.
The file `data/CVE/list` seems to be a master list in a format like:

	[ID]
		<SLUG>[: explanation]
		- [release] package [version|reason] [severity]

[ovalfeed]: https://www.debian.org/security/oval/
[ovalscript]: https://salsa.debian.org/webmaster-team/webwml/-/blob/master/english/security/oval/generate.py
[jsonfeed]: https://security-tracker.debian.org/tracker/data/json
[security-tracker]: https://salsa.debian.org/security-tracker-team/security-tracker/-/tree/master