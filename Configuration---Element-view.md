## Element view configuration

This page explains some features that should be configured for an element (host or service) view.

### Element notes and actions URL

In an element configuration file, it is possible to define `notes`, `notes_url` and `action_url`. 

This is how the Web UI uses these properties.

Each url may be formatted as:
 - url,,description
 - title::description,,url
 - title,,icon::description,,url

description is optional
            
If title is not specified, a default title is used as title
If icon is not specified, a default icon is used as icon

```
   notes                simple note
   notes                Label::note with a label
   notes                KB1023,,tag::<strong>Lorem ipsum dolor sit amet</strong>, consectetur adipiscing elit. Proin et leo gravida, lobortis nunc nec, imperdiet odio. Vivamus quam velit, scelerisque nec egestas et, semper ut massa. Vestibulum id tincidunt lacus. Ut in arcu at ex egestas vestibulum eu non sapien. Nulla facilisi. Aliquam non blandit tellus, non luctus tortor. Mauris tortor libero, egestas quis rhoncus in, sollicitudin et tortor.|note simple|Tag::tagged note ...

   notes_url            http://www.my-KB.fr?host=$HOSTADDRESS$|http://www.my-KB.fr?host=$HOSTNAME$

   action_url           http://www.google.fr|url1::http://www.google.fr|My KB,,tag::http://www.my-KB.fr?host=$HOSTNAME$|Last URL,,tag::<strong>Lorem ipsum dolor sit amet</strong>, consectetur adipiscing elit. Proin et leo gravida, lobortis nunc nec, imperdiet odio. Vivamus quam velit, scelerisque nec egestas et, semper ut massa.,,http://www.my-KB.fr?host=$HOSTADDRESS$

