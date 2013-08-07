<?php


/**
 * Description of BraintreeEventRegistration
 *
 * @author Vivek Arora
 */

require_once 'CiviTest/BaseBraintreeTestCase.php';

class WebTest_Contribute_BraintreeContributionTest extends BaseBraintreeTestCase {

   
    function testAddContributionPageAndDoPayment() {
        $this->open($this->sboxPath);
        $this->webtestLogin();
        $this->waitForPageToLoad("30000");
        $this->_addContributionPage();
    }

    function _addContributionPage() {

        $this->open($this->sboxPath . "civicrm/admin/contribute/add?action=add&reset=1");
        $this->waitForPageToLoad("30000");


        $pageTitle = "PayLeap Contribution Page";
        $this->type('title', $pageTitle . substr(sha1(rand()), 0, 7));
        $this->select('contribution_type_id', 'value=1');
        $this->check('is_active');


        $this->waitForElementPresent('_qf_Settings_next');
        $this->clickAndWait('_qf_Settings_next');
        $this->waitForPageToLoad('3000000');

        $elements = $this->parseURL();
        $pageId = $elements['queryString']['id'];


        $this->waitForElementPresent('_qf_Amount_next');
        $this->waitForElementPresent('access');

        $table = "xpath=//table[@class='form-layout-compressed']/tbody";
        $titleLable = $table . "//tr[@class='crm-contribution-contributionpage-amount-form-block-payment_processor']//th//label[text()='Payment Processor']";
        $checkBox = $table . "//tr//td/label[text()='PayLeap']/../input[@type='checkbox']";

        if (!$this->isElementPresent($titleLable) && !$this->isElementPresent($checkBox)) {
            $this->_addPayLeapPaymentProcessor();
            if (!empty($pageId)) {
                $this->open($this->sboxPath . "civicrm/admin/contribute/amount?action=update&reset=1&id=$pageId");
                $this->waitForPageToLoad('3000000');
            }
        }

        $this->waitForElementPresent($titleLable);
        $this->waitForElementPresent($checkBox);
        $this->check($checkBox);
        //On Amount Block
        $this->waitForElementPresent('_qf_Amount_submit_savenext');
        $this->type('label_1', "Premum");
        $this->type('value_1', 50);


        $this->type('label_2', 'Silver');
        $this->type('value_2', 100);

        $this->type('label_3', 'Gold');
        $this->type('value_3', 200);

        $this->click('_qf_Amount_submit_savenext');
        $this->waitForPageToLoad('30000');

        //On Membership Block

        $this->waitForElementPresent('_qf_MembershipBlock_submit_savenext');
        $this->click('_qf_MembershipBlock_submit_savenext');
        $this->waitForPageToLoad('30000');

        //Thank You Block
        $this->waitForElementPresent('_qf_ThankYou_submit_savenext');
        $this->type('thankyou_title', 'Thanks You');
        $this->click('_qf_ThankYou_submit_savenext');
        $this->waitForPageToLoad('30000');

        //Contribute Block
        $this->waitForElementPresent('_qf_Contribute_submit_savenext');
        $this->click('_qf_Contribute_submit_savenext');
        $this->waitForPageToLoad('30000');

        //Custom Profile Block
        $this->waitForElementPresent('_qf_Custom_submit_savenext');
        $this->click('_qf_Custom_submit_savenext');
        $this->waitForPageToLoad('30000');

        //Premium Block
        $this->waitForElementPresent('_qf_Premium_submit_savenext');
        $this->click('_qf_Premium_submit_savenext');
        $this->waitForPageToLoad('30000');

        //Save and Done

        $this->waitForElementPresent('_qf_Widget_upload_done');
        $this->click('_qf_Widget_upload_done');
        $this->waitForPageToLoad('30000');

        $this->_doContributionPayment($pageId);
    }

    function _doContributionPayment($pageId) {
        //Open Offline Contribution Page
        $this->open($this->sboxPath . "civicrm/contribute/transact?reset=1&id=" . $pageId);
        $this->waitForElementPresent("_qf_Main_upload");
        $contribution_amount_section = "xpath=//div[@class='price-set-row contribution_amount-row2']/span/input[@type='radio']";
        $this->click($contribution_amount_section);


        if ($this->isElementPresent('email-5')) {
            $this->type('email-5', 'reachtobrijesh@gmail.com');
        }

        $this->_setCCAndBillingDetail();

        $this->click("_qf_Main_upload");
        $this->waitForPageToLoad('30000');
        $this->waitForElementPresent('_qf_Confirm_next-bottom');
        $this->click('_qf_Confirm_next-bottom');
        
        $msg = "Your transaction has been processed successfully. Please print this page for your records.";
        $this->waitForTextPresent($msg);
        if (!$this->isTextPresent($msg)) {
            $this->assertTrue(FALSE, 'There is some problem in payment process. Transaction has not been approved.');
        }
    }
 
    function testDoOfflineContributionPayment() {
        $this->open($this->sboxPath);
        $this->webtestLogin();
        $this->waitForPageToLoad("30000");
        $cId = 125;
        $this->_doOfflineContributionPayment($cId);
    }

    function _offlineDoContributionPayment($cId) {
        $url = "civicrm/contact/view/contribution?reset=1&action=add&cid=$cId&context=contribution&mode=live";
        $this->open($url);
    }

}

