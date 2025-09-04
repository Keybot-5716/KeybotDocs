Códigos de FTC
==============

.. tabs::

	.. tab:: **Autonomous.java**

		.. code-block:: java

            package org.firstinspires.ftc.teamcode;

            import com.qualcomm.robotcore.eventloop.opmode.LinearOpMode;
            import com.qualcomm.robotcore.hardware.Servo;
            import com.qualcomm.robotcore.hardware.CRServo;
            import com.qualcomm.robotcore.eventloop.opmode.Autonomous;
            import com.qualcomm.robotcore.hardware.DcMotor;

            @Autonomous
            public class Prueba3 extends LinearOpMode {

                private DcMotor leftDrive = null;
                private DcMotor rightDrive = null;
                private DcMotor arm = null;
                private DcMotor wrist = null;
                private Servo claw = null;
                private CRServo intake = null;

                private static final int ARM_POSITION_INIT = 250;
                private static final int ARM_POSITION_EXTEND = 3815;
                private static final int WRIST_POSITION_INIT = 0;
                private static final int WRIST_POSITION_EXTEND = 265;

                private static final double CLAW_OPEN_POSITION = 0.5;
                private static final double CLAW_CLOSED_POSITION = 1;

                @Override
                public void runOpMode() {
                    leftDrive  = hardwareMap.get(DcMotor.class, "leftDrive");
                    rightDrive = hardwareMap.get(DcMotor.class, "rightDrive");
                    arm = hardwareMap.get(DcMotor.class, "arm");
                    wrist = hardwareMap.get(DcMotor.class, "wrist");
                    claw = hardwareMap.get(Servo.class, "claw");
                    intake = hardwareMap.get(CRServo.class, "intake");

                    // Reset encoders
                    leftDrive.setMode(DcMotor.RunMode.STOP_AND_RESET_ENCODER);
                    rightDrive.setMode(DcMotor.RunMode.STOP_AND_RESET_ENCODER);
                    arm.setMode(DcMotor.RunMode.STOP_AND_RESET_ENCODER);
                    wrist.setMode(DcMotor.RunMode.STOP_AND_RESET_ENCODER);

                    // Motores de drive sin encoder (para mover con potencias)
                    leftDrive.setMode(DcMotor.RunMode.RUN_WITHOUT_ENCODER);
                    rightDrive.setMode(DcMotor.RunMode.RUN_WITHOUT_ENCODER);

                    leftDrive.setDirection(DcMotor.Direction.REVERSE);
                    rightDrive.setDirection(DcMotor.Direction.FORWARD);

                    leftDrive.setZeroPowerBehavior(DcMotor.ZeroPowerBehavior.BRAKE);
                    rightDrive.setZeroPowerBehavior(DcMotor.ZeroPowerBehavior.BRAKE);
                    arm.setZeroPowerBehavior(DcMotor.ZeroPowerBehavior.BRAKE);
                    wrist.setZeroPowerBehavior(DcMotor.ZeroPowerBehavior.BRAKE);

                    telemetry.addData("Status", "Ready for Autonomous");
                    telemetry.update();

                    waitForStart();

                    if (opModeIsActive()) {
                        // --- AUTÓNOMO ---
                        // 1. Cerrar garra
                        claw.setPosition(CLAW_CLOSED_POSITION);
                        sleep(1000);

                        // 2. Extender brazo y muñeca
                        arm.setTargetPosition(ARM_POSITION_EXTEND);
                        arm.setMode(DcMotor.RunMode.RUN_TO_POSITION);
                        arm.setPower(1);

                        wrist.setTargetPosition(WRIST_POSITION_EXTEND);
                        wrist.setMode(DcMotor.RunMode.RUN_TO_POSITION);
                        wrist.setPower(1);

                        sleep(2000);

                        // 3. Abrir garra
                        claw.setPosition(CLAW_OPEN_POSITION);
                        sleep(1000);

                        // 4. Regresar brazo y muñeca
                        arm.setTargetPosition(ARM_POSITION_INIT);
                        wrist.setTargetPosition(WRIST_POSITION_INIT);
                        sleep(2000);

                        // 5. Detener motores
                        arm.setPower(0);
                        wrist.setPower(0);
                        leftDrive.setPower(0);
                        rightDrive.setPower(0);

                        telemetry.addData("Status", "Autonomous Complete");
                        telemetry.update();
                    }
                }
            }



