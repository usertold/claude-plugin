{"studyRef":"checkout-debrief","title":"Checkout usability debrief","interviews":{"completed":5,"failed":1,"abandoned":0},
"interviewSummaries":[
 {"interviewRef":"int_a1","status":"completed","durationS":612},
 {"interviewRef":"int_b2","status":"completed","durationS":540},
 {"interviewRef":"int_c3","status":"completed","durationS":701},
 {"interviewRef":"int_d4","status":"completed","durationS":455},
 {"interviewRef":"int_e5","status":"completed","durationS":388},
 {"interviewRef":"int_f6","status":"error","processing":"failed"}],
"evidence":[
 {"signalId":"sig_1","interviewRef":"int_a1","timestamp_ms":252000,"signal_type":"struggling_moment","quote":"Wait, do I have to pick a shipping speed before it shows me the total? I just want to know what this costs.","observed_behavior":"Scrolled between /checkout/delivery and /cart three times before selecting standard shipping.","interpretation":"Total price is hidden until shipping is chosen.","confidence":0.88,"page":"/checkout/delivery"},
 {"signalId":"sig_2","interviewRef":"int_b2","timestamp_ms":318000,"signal_type":"struggling_moment","quote":"I thought the promo code box would be on this page.","observed_behavior":"Returned to /cart looking for the promo code field, then continued.","interpretation":"Promo code entry is not where users expect it.","confidence":0.81,"page":"/checkout/payment"},
 {"signalId":"sig_3","interviewRef":"int_c3","timestamp_ms":190000,"signal_type":"struggling_moment","quote":"How much is this going to be in the end?","observed_behavior":"Paused 35s on /checkout/delivery, hovered over shipping options.","interpretation":"Total price is hidden until shipping is chosen.","confidence":0.74,"page":"/checkout/delivery"},
 {"signalId":"sig_4","interviewRef":"int_c3","timestamp_ms":205000,"signal_type":"emotional_response","quote":"That's a bit annoying, honestly.","observed_behavior":"Same pause on /checkout/delivery.","interpretation":"Frustration about the hidden total.","confidence":0.6,"page":"/checkout/delivery"},
 {"signalId":"sig_5","interviewRef":"int_d4","timestamp_ms":240000,"signal_type":"smooth_completion","quote":"Oh nice, it estimates shipping already.","observed_behavior":"Completed delivery step in 12s; used saved address.","interpretation":"Returning user with saved address saw an estimated total.","confidence":0.83,"page":"/checkout/delivery"},
 {"signalId":"sig_6","interviewRef":"int_e5","timestamp_ms":410000,"signal_type":"critical_error","quote":"It says my card was declined but it's definitely fine.","observed_behavior":"Payment failed twice with a generic error; participant abandoned the task at /checkout/payment.","interpretation":"Unclear payment error, possibly a 3-D Secure failure.","confidence":0.9,"page":"/checkout/payment"}],
"findings":[],"uncertainty":"5 completed interviews; 1 interview failed processing and has no Evidence. Qualitative sample; counts do not indicate prevalence.","truncated":false}
