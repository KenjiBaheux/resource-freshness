#Freshness#
Proposal for a HTTP header field to be used in the context of stale-while-revalidate (RFC5861)

##Background##
For context, please read [Mark Nottingham's blog post about stale-while-revalidate](https://www.mnot.net/blog/2007/12/12/stale).

##Description of the header##
Freshness is a HTTP header that user agents would send on revalidation of assets that were served with 
the stale-while-revalidate Cache-Control directive.

The purpose of this header is to help webmasters optimize the values of max-age and stale-while-revalidate for a given
resource.

The Freshness header will report the original max-age value, the original stale-while-revalidate value as well
as the current age of the asset.

Example: 
 * Freshness: max-age=86400, stale-while-revalidate=259200, age=103680

##How the header would be used##
Based on the information carried by Freshness, webmasters would be able to know how often user agents had to resort to synchronous
revalidations and how bad the problem is. 

For instance, revalidations could be categorized into asynchronous and synchronous by looking at the following instances:

 * If (age > max-age + stale-while-revalidate) then categorize as a synchronous revalidation
 * If (age <= max-age + stale-while-revalidate) then categorize as an asynchronous revalidation   



Assessing how bad the problem is can be done by looking at the overall efficiency:

 *  # of asynchronous revalidation / (# of synchronous revalidations + # of asynchronous revalidations)

Histograms can be used to understand the impact of Understanding the impact of tweaking max-age and/or stale-while-revalidate.

For instance, one could look at a histogram of beyond-expectation-age to understand by how much stale-while-revalidate
needs to be increased in order to reach a target efficiency number. 
 
 * beyond-expectation-age = age - (max-age + stale-while-revalidate) ; where age > max-age + stale-while-revalidate

