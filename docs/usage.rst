=====
Usage
=====

To use Releases, mimic the format seen in `its own changelog
<https://raw.github.com/bitprophet/releases/master/docs/changelog.rst>`_ or in
`Fabric's changelog
<https://raw.github.com/fabric/fabric/master/docs/changelog.rst>`_.
Specifically:

* Install ``releases`` and update your Sphinx ``conf.py`` to include it in the
  ``extensions`` list setting: ``extensions = ['releases']``.

    * Also set the ``releases_release_uri`` and ``releases_issue_uri`` top
      level options - they determine the targets of the issue & release links
      in the HTML output. Both should have an unevaluated ``%s`` where the
      release/issue number would go.
    * See `Fabric's docs/conf.py
      <https://github.com/fabric/fabric/blob/4afd33e971f1c6831cc33fd3228013f7484fbe35/docs/conf.py#L31>`_
      for an example.
    * You may optionally set ``releases_debug = True`` to see debug output
      while building your docs.

* Create a Sphinx document named ``changelog.rst`` with a top-level header
  followed by a bulleted list.
* List items are to be ordered chronologically with the newest ones on top.

    * As you fix issues, put them on the top of the list.
    * As you cut releases, put those on the top of the list and they will
      include the issues below them.
    * Issues with no releases above them will end up in a specially marked
      "Unreleased" section of the rendered changelog.

* Bullet list items should use the ``support``, ``feature`` or ``bug``
  roles to mark issues, or ``release`` to mark a release. These special roles
  must be the first element in each list item.

    * Line-items that do not start with any issue role will be considered bugs
      (both in terms of inclusion in releases, and formatting) and, naturally,
      will not be given a hyperlink.

* Issue roles are of the form ``:type:`number[ keyword]```. Specifically:
  
    * ``number`` is used to generate the link to the actual issue in your issue
      tracker (going by the ``releases_issue_uri`` option). It's used for both
      the link target & (part of) the link text.
    * If ``number`` is given as ``-`` or ``0`` (as opposed to a "real" issue
      number), no issue link will be generated.  You can use this for items
      without a related issue.
    * Keywords are optional and may be one of:

        * ``backported``: Given on *support* or *feature* issues to denote
          backporting to bugfix releases; such issues will show up in both
          release types. E.g. placing ``:support:`123 backported``` in your
          changelog below releases '1.1.1' and '1.2.0' will cause it to appear
          in both of those releases' lists.
        * ``major``: Given on *bug* issues to denote inclusion in feature,
          instead of bugfix, releases. E.g. placing ``:bug:`22 major``` below
          releases '1.1.1' and '1.2.0' will cause it to appear in '1.2.0'
          **only**.
        * ``(N.N+)`` where ``N.N`` is a valid release line, e.g. ``1.1`` or
          ``2.10``: Given on *bug* issues to denote minimum release line. E.g.
          when actively backporting most bugs to release lines 1.2, 1.3 and
          1.4, you might specify ``:bug:`55 (1.3+)``` to note that bug 55 only
          applies to releases in 1.3 and above - not 1.2.

* Regular Sphinx content may be given after issue roles and will be preserved
  as-is when rendering. For example, in ``:bug:`123` Fixed a bug, thanks
  `@somebody`!``, the rendered changelog will preserve/render "Fixed a bug,
  thanks ``@somebody``!" after the issue link.
* Release roles are of the form ``:release:`number <date>```.

    * You may place a comma-separated (whitespace optional) list of issue
      numbers after the release role, and this will limit the issues included
      in that release to that explicit list.
    * Otherwise, releases include all relevant issues as outlined above and
      in :doc:`/concepts`.

Then build your docs; in the rendered output, ``changelog.html`` should show
issues grouped by release, as per the above rules. Examples: `Releases' own
rendered changelog
<http://releases.readthedocs.org/en/latest/changelog.html>`_, `Fabric's
rendered changelog <http://docs.fabfile.org/en/latest/changelog.html>`_.


Optional styling additions
==========================

If you have any nontrivial changelog entries (e.g. whose description spans
multiple paragraphs or includes their own bulleted lists, etc) you may run into
`docutils' rather enthusiastic bulleted list massaging
<http://docutils.sourceforge.net/sandbox/html4strict/data/simple-lists.html>`_
which can then make your releases look different from one another.

To help combat this, it may be useful to add the following rule to the Sphinx
theme you're using::

    div#changelog > div.section > ul > li > p:only-child {
        margin-bottom: 0;
    }

.. note::
    Some themes, like `Alabaster <http://github.com/bitprophet/alabaster>`_,
    may already include this style rule.
