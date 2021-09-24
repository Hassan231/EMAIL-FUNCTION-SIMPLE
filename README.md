 public void WEAVING_Recievable_Status()
        {
            try
            {
                //if (Check_Mail_Time("Weaving Recievable Status"))
               if(true)
                {
                    Refresh_Controls();
                    Get_Email_List("Weaving Recievable Status", "CML");
                    EmailModule = "Weaving Recievable Status";
                    dt = new DataTable();
                    help.DatabaseName = "CML";
                    Query = @"SELECT * FROM VW_WeavingReceivable ";
                    dt = help.Return_DataTable_Query(Query);

                    if (dt.Rows.Count > 0)
                    {
                        EmailMsg = "<style> th,td {text-align:center;}tr:nth-child(even) {background-color: #f2f2f2;}.text-left{text-align:left;}.text-right{text-align:right;}</style>";
                        double total = 0;
                        EmailSubject = "Weaving Recievable Status(Over Due Date) By " + DateTime.Now.ToString("dd-MMM-yyyy");
                        EmailMsg += "<table style='width:60%;' cellspacing='1' cellpadding='1' border='1'>";
                        EmailMsg += "<tr><td colspan='5' style='background-color:#0b72ba; height: 50px; color:white;text-align:center;'><b>" + EmailSubject + "</td></tr>";
                        EmailMsg += "<tr style='background-color:#c8dbec;'><th>#</th><th>Customer ID</th><th class='text-left'>Customer Name</th><th>Amount(Over Due Date)</th><th width='50%'>Remarks</th></tr>";
                        for (int i = 0; i < dt.Rows.Count; i++)
                        {
                            EmailMsg += "<tr>";
                            EmailMsg += "<td>" + i + "</td>";

                            EmailMsg += "<td class='text-left'>" + dt.Rows[i]["CUSTNMBR"].ToString() + "</td>";
                            //added design class in Fields 
                            EmailMsg += "<td class='text-left'>" + dt.Rows[i]["CUSTNAME"].ToString() + "</td>";
                            EmailMsg += "<td class='text-right'>" + Convert.ToDecimal(dt.Rows[i]["Amount"]).ToString("n0") + "</td>";

                            EmailMsg += "<td>" + dt.Rows[i]["Remarks"] + "</td>";
                            EmailMsg += "</tr>";
                            total +=  double.Parse(dt.Rows[i]["Amount"].ToString().Trim());
                        }
                        //Getting Data Line Wise Through LOOP........
                        
                        EmailMsg += "<tr style='background-color:#1f4e78; color:white;' class='text-right'>";
                        EmailMsg += "<th colspan='3'>Total</th><th class='text-right'>" + total+ "</th><th></th></tr>";
                        
                        EmailMsg += "</table>";
                        Generate_EmailFooter_HLR(40);
                        EmailMsg += EmailFooter;
                        //SendEmail_HLR(EmailMsg);
                        SendEmail(EmailMsg);
                        Check_Schedule_Email("Weaving Recievable Status");
                        //=====Update IsMailed==========
                    }
                }
            }
            catch (Exception ex)
            {
                App_Log("GP Mailer Service (Weaving Recievable Status Email Error) :" + EmailModule + ex.Message);
                help.DatabaseName = "DYNAMICS";
                help.ExecuteParameterizedProcedure("UD_Error_ForMail", "@DocNo,@Company,@Module,@Error", "", "CML", EmailModule, ex.Message);
            }
        }
