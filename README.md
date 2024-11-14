function onChange(control, oldValue, newValue, isLoading) {
    if (isLoading || newValue === oldValue) {
        return;
    }
    var ga = new GlideAjax('VendorEngagementClientUtlis'); // Call your Script Include
    ga.addParam('sys_id', newValue); // Send the selected supplier's sys_id to the Script Include
    ga.addParam('sysparm_name', 'getEngagementsForSupplier');
    ga.getXMLAnswer(function(response) {
        var engagements = JSON.parse(response.responseXML.documentElement.getAttribute("answer"));
        // Clear and populate the engagement field options
        g_form.clearOptions('if_it_is_a_reassessment_of_an_existing_engagement_please_select_the_eng_you_are_reassessing'); // Replace with your engagement field's name
        g_form.addOption('if_it_is_a_reassessment_of_an_existing_engagement_please_select_the_eng_you_are_reassessing', '', '--Select Engagement--'); // Add a default option
        // Populate engagements based on response
        engagements.forEach(function(engagement) {
            g_form.addOption('if_it_is_a_reassessment_of_an_existing_engagement_please_select_the_eng_you_are_reassessing', engagement.sys_id, engagement.name);
        });
    });
}
