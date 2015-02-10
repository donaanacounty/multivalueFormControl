# multivalueFormControl: Alfresco Multi Value Field Form Control Template

Properties within the Alfresco model can be defined to hold multiple values.  This can be very useful if you want a many-to-one relationship.  Unfortunately, the Alfresco Share web GUI doesn't provide a very useful control to manipulate multi-valued fields.  It only provides a simple text input. Any commas in the text input will become separators and the content between the commas will will be interpreted as the values.  So entering "a,c,b" (without quotes) into the text field will be interpreted as three values "a", "b" and "c".

This Form control template takes advantage of this feature.  Rather than trying to parse anything at the other end, provides a GUI to add and remove fields, and the content of the visible fields are copied into a hidden field using browser side javascript.  When the form is submitted, the visible fields are ignored by Alfresco (it will spit some warning into the log that unrecognized fields are being submitted) and the invisible field will have the value that is actually submitted.

In many ways, this is a bit of a hack, but it works, until Alfresco decides to give us a proper form control for multiple values.  It has three main limitations at the moment.

- First, it only works with text fields.  No date pickers, or other alfresco controls.  Also there is no content validation, so if you use it for d:int, it will happily attempt to submit.  I don't know how it will behave.
- Second, I have done no testing with error handling.  So, if you put this on a mandatory field and submit empty I don't know what happens.  If you put it on an int, and type text, I don't know what happens.
- Finally, because this is only a form template, and (aside from the FTL parts) runs on the client browser, commas are used for divisions between values on submission.  You don't see them but they are applied automatically to the hidden form field.  This means that you cannot have commas in your data.  There may be a way around that by encoding the data before submission, but so far I haven't found it so instead I don't allow the user to type commas.


Usage is easy.  First you have to install the amp into your share.war.  Second define some property to allow multi values:</p>

```
<property name="co:someCoList">
  <type>d:text</type>
  <multiple>false</multiple>
</property>
```

Next when setting up the form (probably in share-config-custom.xml) tell share to use this control:

```
<control template="/org/donaanacounty/components/form/controls/multiple.ftl" />
```
