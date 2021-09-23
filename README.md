# EMAIL-FUNCTION-SIMPLE

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
                        EmailMsg = "<style> th,td {text-align:center;}tr:nth-child(even) {background-color: #f2f2f2;}</style>";

                        EmailSubject = "Weaving Recievable Status[Over Due Date] By " + DateTime.Now.ToString("dd-MMM-yyyy");
                        EmailMsg += "<table style='width:40%;' cellspacing='1' cellpadding='1' border='1'>";
                        EmailMsg += "<tr><td colspan='4' style='background-color:#0b72ba; color:white;text-align:center;'><b>" + EmailSubject + "</td></tr>";
                        EmailMsg += "<tr><th>Customer ID</th><th>Customer Name</th><th>Amount[Over Due Date]</th><th>Remarks</th></tr>";
                        for (int i = 0; i < dt.Rows.Count; i++)
                        {
                            EmailMsg += "<tr>";
                            EmailMsg += "<td>" + dt.Rows[i]["CUSTNMBR"].ToString() + "</td>";

                            EmailMsg += "<td>" + dt.Rows[i]["CUSTNAME"].ToString() + "</td>";
                            EmailMsg += "<td>" + Convert.ToDecimal(dt.Rows[i]["Amount"]).ToString("n1") + "</td>";

                            EmailMsg += "<td>" + dt.Rows[i]["Remarks"] + "</td>";
                            EmailMsg += "</tr>";
                        }
                        EmailMsg += "</table>";
                        Generate_EmailFooter_HLR(40);
                        EmailMsg += EmailFooter;
                        //SendEmail_HLR(EmailMsg);
                        //SendEmail(EmailMsg);
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
